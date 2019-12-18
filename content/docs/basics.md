---
# Course title, summary, and position in the list.
linktitle: An Example Course
summary: Learn how to accelerate you 16S rRNA analysis with MIKIbiomeR.
weight: 10

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
    weight: 1
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
data_load(map = "mapping_file_path.biom",
          otu = "otu_file_path.biom",
          tree = "tree_file_path.tree)
```

This will create a 'phyloseq object' from the
[phyloseq package](https://joey711.github.io/phyloseq).

Example

```r
phylo <- data_load(map = "data/mapping_file_path.biom",
                   otu = "data/otu_file_path.biom",
                   tree = "data/tree_file_path.tree)
```

There are also some example data shipped with the pacakge.
