---
layout: post
title:  "Multiple regression lines in ggpairs" 
date:   2016-02-16 23:00:00
comments: true
categories: 
- R
- GGally
- ggplot2
- ggpairs
tags:
- R
- GGally
- ggplot2
- ggpairs

abstract: Plots including multiple regression lines are added to a matrix of plots
  generated with the GGally package in R.
---

## Abstract

{{ page.abstract }}[^1]

[^1]: Credit to an excellent [answer on Stack Overflow](http://stackoverflow.com/a/35088740/4257137).

## Background 

Built upon [ggplot2][ggplot2], [GGally][GGally] provides templates
for combining plots into a matrix through the `ggpairs` function. 
Such a matrix of plots can be useful for quickly exploring the 
relationships between multiple columns of data in a data frame. 
The `lower` and `upper` arguments to the `ggpairs` function specifies
the type of plot or data in each position of the lower or upper diagonal
of the matrix, respectively. 
For continuous X and Y data, one can specify the `smooth` option to
include a regression line.
To include more than one regression line in a plot (or to customize the
plot in any way beyond one of the predefined types), one simply
defines a function that returns the desired plot. 
The function can then be included in the list provided to `upper` or
`lower`.

## The code

The following code demonstrates inclusion of two regression lines
in each plot in the lower diagonal elements in which both X and Y
data are continuous. 

{% gist 3ce8174228d5607b78be %}

## The plot

The plot generated by the above code is included here. 
Note that the default transparency of the
confidence interval works fairly well for being able to compare
the two types of regression curves.

![Plot generated by above code](
{{ "/assets/multiple_smooth_ggpairs.png" | prepend: site.baseurl | prepend: site.url }})

[ggplot2]:      http://ggplot2.org
[GGally]:       https://github.com/ggobi/ggally
