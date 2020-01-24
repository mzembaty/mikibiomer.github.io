---
# Course title, summary, and position in the list.
linktitle: An Example Course
summary: Learn how to accelerate you 16S rRNA analysis with MIKIbiomeR.
weight: 20

# Page metadata.
title: "Basics"
date: "2019-12-17"
lastmod: "2019-12-17"
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
math: true

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
  docs:
    name: "Basics"
    weight: 20
---

Here you will learn the fundamentals of MIKIbiomeR.

## Projects

RStudio has a great way to create easy to share distinct analysis,
[Projects](https://rafalab.github.io/dsbook/reproducible-projects-with-rstudio-and-r-markdown.html#rstudio-projects).
As a rule of thumb, every experiment should have it's own project.
It's also good pratice to keep a seperate folder only for your raw data.
After you created a new Project, create a new folder called 'data' in
your project folder and copy your raw data into it.

MIKIbiomeR needs 3 files.

- mapping file
- otu count table
- tree file

Now create a new file for you code. This can be either a R script or a
Rmarkdown file. You can also use MIKIbiomeR's handy template, since most
16S rRNA analysis share a common structure.

To use MIKIbiomeR in the new script, first thing to do is to load the
library in R:

```r
library("MIKIbiomeR")
```

## Loading Data
{{% alert note %}}
In the whole documentation a dataset will be used, which compares the
microbiome in different mice. These mice come from different vendors.
{{% /alert %}}

Then we load raw data into R using MIKIbiomeR's `data_load()`:

```r Loading data
data_load(map = "mapping_file_path.txt",
          otu = "otu_file_path.biom",
          tree = "tree_file_path.tree)
```

This will create a 'phyloseq object' from the
[phyloseq package](https://joey711.github.io/phyloseq).

If your raw data is stored in a folder called 'data',
you would point to the location like this:

```r
phylo <- data_load(map = "data/mapping_file.txt",
                   otu = "data/otu_file.biom",
                   tree = "data/tree_file.tree)
```

There are also some example data shipped with the package,
which can be accessed with `system.path()` from base r.

```r load examples
# get example data path
mapfile_path <- system.file("MapFile.txt", package = "MIKIbiomeR")
otu_table_path <- system.file("final_otutable_tax.biom", package = "MIKIbiomeR")
tree_path <- system.file("Rep_Set_tree.tree", package = "MIKIbiomeR")

# load data from path
phylo <- data_load(map = mapfile_path,
                   otu = otu_table_path,
                   tree = tree_path)
```

There is also a ready to use example phyloseq object,
which can be simply load with `data()`.

```r load example data
phylo <- data(mice_B6_N)
```
If everything worked correctly the imported data should be converted to a
phyloseq object. Printing the object would result in something similiar to
this:

```r print phyloseq object
# printing the object by typing out its name
phylo

# result
phyloseq-class experiment-level object
otu_table()   OTU Table:         [ 453 taxa and 166 samples ]
sample_data() Sample Data:       [ 166 samples by 16 sample variables ]
tax_table()   Taxonomy Table:    [ 453 taxa by 7 taxonomic ranks ]
phy_tree()    Phylogenetic Tree: [ 453 tips and 452 internal nodes ]
```

## $\alpha$-Diversity
MIKIbiomeR calculates 7 different $\alpha$-diversity distances
(Observed, Chao1, ACE, Shannon, Simpson, InvSimpson,BF_ratio).
BF_ratio calculates the bacteroides to firmicutes ratio.

`alpha_div_calc()` calculates all the $\alpha$-diversity distances at once and
adds the information to the sample data of the phyloseq object.

```r calculate alpha-diversity
phylo_alpha <- alpha_div_calc(phylo)

# check results
sample_data(phylo_alpha)
```
|       | Observed|     Chao1|  se.chao1|       ACE|   se.ACE|  Shannon|   Simpson| InvSimpson|  BF_ratio|
|:------|--------:|---------:|---------:|---------:|--------:|--------:|---------:|----------:|---------:|
|100L19 |       93|  98.83333|  4.164971| 104.18753| 5.056604| 3.431708| 0.9424020|   17.36171| 0.2404653|
|101L19 |       88| 118.66667| 16.940788| 109.55210| 5.177028| 3.081947| 0.9152860|   11.80442| 0.2206887|
|102L19 |       82|  96.25000|  8.653266|  99.59932| 5.074948| 3.158823| 0.9256140|   13.44339| 0.1564885|
|103L19 |       82|  94.00000|  7.959739|  93.20114| 4.761550| 3.119463| 0.9242470|   13.20080| 0.2095097|
|104L19 |       83|  88.57143|  4.240918|  91.19668| 4.625123| 3.250927| 0.9324255|   14.79848| 0.2392315|
|105L19 |       84|  97.90909|  8.683968|  98.07586| 4.851067| 3.185105| 0.9288165|   14.04820| 0.2679954|

To plot the distances use either `alpha_div_plot()` to create only one plot for
one distance method or `alpha_div_plots()` to create all plots at once and
output them as a `list`.

```r one alpha plot
alpha_div_plot(analysis_alpha, "Vendors", color_var = "Vendors",
               alpha_dist = "BF_ratio")
```
```r alpha plots
alpha_div_plots(analysis_alpha, "Vendors", color_var = "Vendors")
```

{{% alert note %}}
Almost all plots created in MIKIbiomeR are made with ggplot2. They can be
adjusted as well as any other ggplot2 object.

In [Customize Plots]({{< relref "customize_plots" >}}) are frequently asked
customizations, as well as some hints how to use certain parameters.
{{% /alert %}}


## $\beta$-Diversity
How different is the microbial composition in one environment compared to
another? Beta diversity shows the difference between microbial communities
from different environments. Main focus is on the difference in taxonomic
abundance profiles from different samples.

{{% alert note %}}
Before going on we need to normalize the OTU table to the same sample depth.

TODO: elaborate here
{{% /alert %}}

### Ordination & Adonis
To plot multivariate data, ordination is often used. It orders multivariate
data in such a way, that similar data is close to each other and dissimilar are
further from each other.

Adonis performs a MANOVA and is an alternative aproach to ordination. It also
tries to explain how variation is attributed to different experimental
treatments or uncontrolled covariates.

With `beta_ordination()` several computations are combined in one function.
$\beta$-diversity measurements, ordination matrix and adonis are calculated and
then plotted together. Additionally adonis calculation results are printed to
the console. Over the ordination scatterplot is an optional star plot plotted.

```r ordination plot
beta_ordination(phylo, distance_method = "bray",
                 formula = "Genotype + Experiment + Cage",
                 ordination_type = "NMDS",
                 color = "Genotype")
```

### Direct distance comparison
To compare $\beta$-diversity distances between samples directly
`beta_dist_calc()` should be used and the results plotted with
`beta_dist_plot()`.

```r beta distance
beta_dist <- beta_dist_calc(phylo, var = "Genotype", dist = "unifrac")
beta_dist_plot(beta_dist)
```

### Abundance plot
`abundance_plot()` creates a stacked bar plot of taxa abundance.

The parameter `taxlev` sets to which taxonomic level the taxa should be summed
up to. The parameter `ntaxa` sets how many of the most abundand taxa should be
displayed individually, the others will be summed up as a group called
"Others".

```r abundance plot
abundance_plot(phylo = phylo, "Vendor", facetby = "Experiment")
```

Relative abundances can be calculated with `calc_rel_abund()`, which in turn
can be plotted.

```r relative abundance
phylo_rel <- calc_rel_abund(phylo)
abundance_plot(phylo = phylo_rel, "SampleID", facetby = "Experiment")
```
<hr>

{{% alert warning %}}
The functions in the topics below are likely to change in the near future!
{{% /alert %}}

## Differential Abundance

Right now MIKIbiomeR supports DESeq2 to calculate differential abundance.

### DESeq2
DESeq2 can compare multivariate data. `deseq2_plot()` creates a MA-plot, which
shows the $log_2$ fold changes attributable to a given variable over the mean of
normalized counts for all OTUs.

```r deseq
deseq2_res <- MIKIbiomeR::deseq2_analysis(phylo, "Vendor", alpha = 0.05)
deseq2_plot(deseq2_res, title = "HZI vs N6", label_taxlev = "Family")
```

### ALDEx2
ALDEx2 can only compare 2 variables with each other.

```r aldex
aldex2_res <- ALDEx2::aldex(phylo@otu_table, as.character(sample_data(phylo)$Genotype))
ALDEx2::aldex.plot(aldex2_res, type = "MA", test = "welch")   
ALDEx2::aldex.plot(aldex2_res, type="MW", test="welch")
```

### LEfSe

Coming soon


### Random Forest
Random forest is a machine learning technique. In MIKIbiomeR it is used to
produce a classification model. The resulting model gives hints on, which
OTUs can explain the best the differences between defined groups.

```r random forest
model <- classify_randomForest(phylo, "Vendor")
plot_randomForest_results(model, phylo, "Vendor", top = 5)
```
