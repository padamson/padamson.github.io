---
layout: post
title:  "R code to accompany Real-World Machine Learning (Chapter 3)" 
date:   2016-10-15 22:00:00
comments: true
categories: 
- R
- ggplot2
- dplyr
- caret
tags:
- R
- ggplot2
- dplyr
- caret

abstract: The rwml-R Github repo is updated with R code to accompany 
  Chapter 3 of the book "Real-World Machine Learning" by Henrik Brink, 
  Joseph W. Richards, and Mark Fetherolf. 
---

## Abstract

{{ page.abstract }}

## Survivors on the Titanic

The Titanic Passengers dataset is used to illustrate various processes used
to prepare data for modeling, including 
conversion of `factor` variables to dummy variables. For example, the code
to produce the
following table of processed data is provided:

|Survived.yes | Pclass| Sex.male| Age| SibSp| Parch| Embarked.Q| Embarked.S| sqrtFare|
|:------------|------:|--------:|---:|-----:|-----:|----------:|----------:|--------:|
|0            |      3|        1|  22|     1|     0|          0|          1| 2.692582|
|1            |      1|        0|  38|     1|     0|          0|          0| 8.442944|
|1            |      3|        0|  26|     0|     0|          0|          1| 2.815138|
|1            |      1|        0|  35|     1|     0|          0|          1| 7.286975|
|0            |      3|        1|  35|     0|     0|          0|          1| 2.837252|
|0            |      3|        1|  -1|     0|     0|          1|          0| 2.908316|

I also go "off-script" a bit (do some things not contained in the book) and 
demonstrate some useful visualization, modeling, and performance 
measuring techniques available with the 
[`caret`][caret] and [`AppliedPredictiveModeling`][apm] packages.

## MNIST database of handwritten digits

A k-nearest neighbors classifier (from the `kknn` package) is used to
predict the numbers represented in the MNIST database of handwritten digits.
Examples of the types of digits present in the dataset and the R code to
display them:

{% gist 1cdf6e430a1fb82c096539188833bacf %}

![Figure generated by above code](
{{ "/assets/figure3_10.png" | prepend: site.baseurl | prepend: site.url }})

## Auto MPG dataset

As an example of a linear regression analysis, the Auto MPG dataset introduced 
in Chapter 2 resurfaces and fuel economy is predicted from origin, year of 
production, and performance characteristics such as horsepower and engine
displacement.

## As always, feedback is welcome

As always, I'd love to hear from you if you find the project helpful or if you 
have any suggestions. Please leave a comment below or use the Tweet button.
Also, feel free to fork the [rwml-R repo][rwml-R]
and submit a pull request if you wish to contribute.

<a class="github-button" href="https://github.com/padamson/rwml-R/archive/master.zip" data-icon="octicon-cloud-download" data-style="mega" aria-label="Download padamson/rwml-R on GitHub">Download</a>
<a class="github-button" href="https://github.com/padamson/rwml-R/fork" data-icon="octicon-repo-forked" data-style="mega" data-count-href="/padamson/rwml-R/network" data-count-api="/repos/padamson/rwml-R#forks_count" data-count-aria-label="# forks on GitHub" aria-label="Fork padamson/rwml-R on GitHub">Fork</a>

[caret]:        http://topepo.github.io/caret/
[apm]:          https://cran.r-project.org/web/packages/AppliedPredictiveModeling/index.html
[rwml-R]:       https://padamson.github.io/rwml-R/
