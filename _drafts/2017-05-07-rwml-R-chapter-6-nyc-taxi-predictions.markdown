--- 
layout: post
title:  "R code to accompany Real-World Machine Learning (Chapter 6): Making Predictions" 
date:   2017-05-07 23:00:00
comments: true
categories: 
- R
- ggplot2
- gridExtra
- dplyr
- caret
- ranger
tags:
- R
- ggplot2
- gridExtra
- dplyr
- caret
- ranger

abstract: In the latest update to the rwml-R Github repo, R code is provided to 
  complete the analysis of New York City taxi data
  from Chapter 6 of the book "Real-World Machine Learning" by 
  Henrik Brink, Joseph W. Richards, and Mark Fetherolf. Examples given include 
  exploratory data analysis with `ggplot2` to identify and remove bad data; 
  scaling numerical data to remove potential bias; 
  development of machine learning models with `caret`; 
  plotting of receiver operating characteristic (ROC) curves to evaluate
  model performance; and tabulation and display of feature importance using 
  `caret` and `gridExtra`.
---

## Abstract

{{ page.abstract }}

## Data import and EDA for NYC taxi example

As mentioned in [my previous post][prevPost], 
[the complete code-through for Chapter 6][chap6] includes
examples of getting the data and performing exploratory data analysis (EDA).

## Omitting bad data

A plot of the `tip_amount` data reveals a large number of rides that 
result in no tip. Further analysis shows that these "no tip" rides are
associated with cash payers. In order to develop a model to predict 
tipping behavior based on other factors, we must remove the data for
which `payment_type == "CSH"`. This is done easily with the `filter`
function in `dplyr`. The remaining data is then plotted in a 
histogram with `ggplot2`. 

{% highlight R %}
dataJoined <- dataJoined %>%
  filter(payment_type != "CSH")
p9 <- ggplot(dataJoined, aes(tip_amount)) +
  geom_histogram() +
  scale_x_continuous(limits = c(-1, 16.0), name = "Tip amount (USD)") +
  scale_y_continuous(name = "Count")
p9
{% endhighlight %}

![The distribution of tip amounts when omitting cash payments](
{{ "/assets/figure6.6-1.png" | prepend: site.baseurl | prepend: site.url }}) 

## Model development and evaluation

![The ROC curve and feature importance list for the random forest model, including all categorical features and additional date-time features](
{{ "/assets/figure6.11b-1.png" | prepend: site.baseurl | prepend: site.url }}) 


## Feedback welcome 

If you have any feedback on the [rwml-R][rwml-R-gh] project, please
leave a comment below or use the Tweet button.
As with any of my projects, feel free to [fork the rwml-R repo][rwml-R-fork]
and submit a pull request if you wish to contribute.
For convenience, I've created a [project page for rwml-R][rwml-R] with 
the generated HTML files from `knitr`, including a page with 
[all of the event-modeling examples from chapter 6][chap6].

<a class="github-button" href="https://github.com/padamson/rwml-R/archive/master.zip" data-icon="octicon-cloud-download" data-style="mega" aria-label="Download padamson/rwml-R on GitHub">Download</a>
<a class="github-button" href="https://github.com/padamson/rwml-R/fork" data-icon="octicon-repo-forked" data-style="mega" data-count-href="/padamson/rwml-R/network" data-count-api="/repos/padamson/rwml-R#forks_count" data-count-aria-label="# forks on GitHub" aria-label="Fork padamson/rwml-R on GitHub">Fork</a>

[rwml-R-gh]:    https://github.com/padamson/rwml-R
[rwml-R]:       https://padamson.github.io/rwml-R/
[rwml-R-fork]:  https://github.com/padamson/rwml-R/fork
[chap6]:        https://padamson.github.io/rwml-R/Chapter6.html
[prevPost]:     https://padamson.github.io/r/ggplot2/dplyr/fread/2017/04/22/rwml-R-chapter-6-nyc-taxi-eda.html
