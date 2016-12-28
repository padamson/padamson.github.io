--- 
layout: post
title:  "R code to accompany Real-World Machine Learning (Chapters 2-4 Updates)" 
date:   2016-12-28 06:00:00
comments: true
categories: 
- R
- ggplot2
- dplyr
- caret
- doParallel

tags:
- R
- ggplot2
- dplyr
- caret
- doParallel

abstract: I updated the R code to 
  accompany Chapter 2-4 of the book "Real-World Machine Learning" by 
  Henrik Brink, Joseph W. Richards, and Mark Fetherolf to be more
  consistent with the listings and figures as presented in the book. 
---

## Abstract

{{ page.abstract }}

## rwml-R Chapters 2-4 updated

The most notable changes to [rwml-R][rwml-R] are for [Chapter 4][chap4], where 
multiple ROC curves are 
plotted for a 10-class classifier and a tile plot is generated for
a tuning parameter grid search.
Also, for parallel computations, the [`doMC`][doMC] package was replaced with 
[`doParallel`][doParallel]. 

## Plotting a series of ROC curves 

To be consistent with the approach followed in the book, I've added listings 
of R code to compute the 
ROC curves and AUC values "from scratch" instead of using the `ROCR`
package as was done previously:

{% gist 8d9d45fd582695649528085b19b5675f %}

## Tuning model parameters in Chapter 4

The [`caret`][caret] package is used to tune parameters via grid search
for the Support Vector Machines model with a Radial Basis Function Kernel. 
By setting `summaryFunction = twoClassSummary`
in `trainControl`, the `ROC` curve is used to select the optimal 
model. For consistency with the book, tile plots were added to illustrate the 
process of refining 
the grid for the parameter search. The tile plot for the second (refined)
grid search is below.

![Tile plot for parameter search](                                                                                      
{{ "/assets/figure4.25sub2-1.png" | prepend: site.baseurl | prepend: site.url }}) 

## Feedback welcome 

If you have any feedback on the [rwml-R][rwml-R] project, please
leave a comment below or use the Tweet button.
As with any of my projects, feel free to [fork the rwml-R repo][rwml-R-fork]
and submit a pull request if you wish to contribute.
For convenience, I've created a [project page for rwml-R][rwml-R] with 
the generated HTML files from `knitr`. 

<a class="github-button" href="https://github.com/padamson/rwml-R/archive/master.zip" data-icon="octicon-cloud-download" data-style="mega" aria-label="Download padamson/rwml-R on GitHub">Download</a>
<a class="github-button" href="https://github.com/padamson/rwml-R/fork" data-icon="octicon-repo-forked" data-style="mega" data-count-href="/padamson/rwml-R/network" data-count-api="/repos/padamson/rwml-R#forks_count" data-count-aria-label="# forks on GitHub" aria-label="Fork padamson/rwml-R on GitHub">Fork</a>

[caret]:        http://topepo.github.io/caret/
[rwml-R]:       https://padamson.github.io/rwml-R/
[rwml-R-fork]:  https://github.com/padamson/rwml-R/fork
[doMC]:         https://cran.r-project.org/web/packages/doMC/index.html
[doParallel]:   https://cran.r-project.org/web/packages/doParallel/index.html
[chap4]:        https://padamson.github.io/rwml-R/Chapter4.html
