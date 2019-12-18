---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Update 0.0.0.2"
subtitle: "1st Refactoring"
summary: ""
tags: []
categories: []
date: 2019-12-18T15:17:54+01:00
lastmod: 2019-12-18T15:17:54+01:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

No features were added in this patch. The functions do basically the same. Instead
a consistent naming convention was introduced to the functions. Parameters have a
more consistent naming and order. Responsibility of functions was reduced,
meaning that one function should handle as few tasks as possible. Therefore some
new functions were created. Furthermore the use of `return()` will from now on
be [discouraged](https://stackoverflow.com/questions/11738823/explicitly-calling-return-in-a-function-or-not#11834490).

In the following the particular changes are listed.

## New functions

- `alpha_div_plot()` _plots only one alpha distance_
- `adonis_plot()` _plots adonis results_
- `ordination_plot()` _plots ordination_

## Changed function names

- alpha_diversity_plus_dt() -> alpha_div_calc()
- alpha_diversity_plus_plot -> alpha_div_plots()
- get_beta_distance() -> beta_dist_calc()
- get_beta_distance_plot() -> beta_dist_plot()
- facet_barplot() -> abundance_plot()

## Specific function changes

### data_load()

*Parameter changed:*

- "OTUTABLE" is now "otu"
- "MAPPINGFILE" is now "map"
- "TREE" is now "tree"
- additional "verbose" boolean parameter.  If TRUE print step progress.

### alpha_div_calc()

*Parameter changed:*

- uses "BF_ratio" instead of "bf_ratio"

### beta_dist_calc()

*Parameter changed:*

- instead of "s", "sample_ID" is used
- standard parameter value of dist, describing the distance method, is removed.
Since this is an imported decision and users should choose the fitting value.

Added `suppressWarnings()` to inhibit warning for type conversion in S4 objects.
To avoid this warning one has to write considerable amount of code, not changing
the outcome.

### get_beta_distances_plot()

*Parameter changed:*

- x and y removed. Comparison and value is always plotted. No parameters needed

### Implicit package use

Most of the code style `package::function` was dropped in favor of
`@importFrom package function` style at the start of the function.
This makes the code much more readable, but is a little unsafer,
since the functions package is not explicitly declared.
Many errors poped up _only_ after `devtools::install()` was used. A lot had to
do with some wierd bug, in which one cannot write the following:

```r
@importFrom package function
function(x) <- foo
```

One had to write `package::function(x) <- foo`

## Packages

- replaced *egg* with *patchwork*

## Bugs found

- in `alpha_div_calc()` an additional X is introduced in the SampleIDs.
  This issue was bypassed using regexp, which is extremely dangerous!
  See issue #14 for more on this subject. I don't know how to solve this yet.
- See issue #15 abundance_plot() only plots relative abundance when x=SampleID.
