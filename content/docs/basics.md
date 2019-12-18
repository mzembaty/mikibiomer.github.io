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

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
  docs:
    name: "Get Started"
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

```r
# get example data path
mapfile_path <- system.file("data-raw", "MapFile.txt", package = "MIKIbiomeR")
otu_table_path <- system.file("data-raw", "final_otutable_tax.biom", package = "MIKIbiomeR")
tree_path <- system.file("data-raw", "Rep_Set_tree.tree", package = "MIKIbiomeR")

# load data from path
phylo <- data_load(map = mapfile_path,
                   otu = otu_table_path,
                   tree = tree_path)
```

There is also a ready to use example phyloseq object,
which can be simply load with `data()`.

```r
phylo <- data(mice_B6_N)
```
