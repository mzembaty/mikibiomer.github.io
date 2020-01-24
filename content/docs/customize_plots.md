---
# Course title, summary, and position in the list.
linktitle: An Example Course
summary: Learn how to customize MIKIbiomeR plots.
weight: 30

# Page metadata.
title: "Customize Plots"
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
    name: "Customize Plots"
    weight: 30
---

## beta_ordination()
This function performs ordination and an adonis test and plots the results.
Additionally it prints the results of the adonis test.

### Ordination calculations should be printed
If the calculations performed by the ordination should be calculated aswell,
set the parameter `verbose = TRUE`.

```r print ordination calculation
beta_ordination(phylo, distance_method = "bray",
                 formula = "Genotype + Experiment + Cage",
                 ordination_type = "NMDS",
                 color = "Genotype",
                verbose = TRUE)
```

### Plot only ordination
If only the ordination plot should appeare, the parameter `formula` can be left
out.

```r Only Ordination plot
beta_ordination(phylo, distance_method = "bray",
                 ordination_type = "NMDS",
                 color = "Genotype")
```

### No connections between points
By default `beta_ordination()` creates a star plot on top of the ordination
plot. To prevent this from happening set the parameter `geom_star = FALSE`

```r Only Ordination plot
beta_ordination(phylo, distance_method = "bray",
                 ordination_type = "NMDS",
                 color = "Genotype")
```

## abundance_plot()


### Known issues

#### Stacked percentage bars
```r issue 1
abundance_plot(phylo = phylo_rel, "Mouse", facetby = "Genotype", taxlev = "Family", ntaxa = 15)
abundance_plot(phylo = phylo_rel, "Genotype", facetby = "Genotype", taxlev = "Family", ntaxa = 15)
```
Creates a bar plot with stacked percentages

<hr>

#### Ordering sometimes creates strange outputs