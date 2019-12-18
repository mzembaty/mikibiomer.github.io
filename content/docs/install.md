---
# Course title, summary, and position in the list.
linktitle: Installation Guide
summary: Learn how to install MIKIbiomeR.
weight: 10

# Page metadata.
title: "Install"
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
    name: "Install"
    weight: 10
---

## R and RStudio

If you don't have R and RStudio installed go to the
[RStudio website](https://rstudio.com/products/rstudio/download/#download)
and download the right version for your operating system (Windows, Mac, Linux).

If you need an introduction using R there are several free resources on the web.
Here is a small selection:

- [R for Data Science by *Hadley Wickham*](https://r4ds.had.co.nz/)
- [The R Programming Environment - Coursera](https://www.coursera.org/learn/r-programming-environment)
- [Youtube Course by *David Langer*](https://www.youtube.com/watch?v=32o0DnuRjfg&list=PLTJTBoU5HOCRrTs3cJK-PbHM39cwCU0PF&index=1)

## MIKIbiomeR

Since MIKIbiomeR is still in developement and only accesible via Github,
a special package is needed to install it. In the R console
install 'devtools' with the following code:

```r
install.packages("devtools")
```

After devtools is installed MIKIbiomeR can be downloaded from the Github
repository. Keep in mind, that the package is not published yet, therefore
the access is limited to the MIKI Lab, who have the secrete token.You
install the package via the following command:

```r
devtools::install_github(repo = "mzembaty/MIKIbiomeR",
                        auth_token = "copy_secrete_token_here",
                        force = TRUE)
```

If you are using a rmarkdown file, keep an eye on your terminal, when
installing a new package. Sometimes R needs to make updates to some
existing packages and will ask for input, but this shows only in the
terminal not in the output panel.
