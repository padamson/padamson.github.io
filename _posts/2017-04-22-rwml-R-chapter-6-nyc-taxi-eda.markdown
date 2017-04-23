--- 
layout: post
title:  "R code to accompany Real-World Machine Learning (Chapter 6): Exploring NYC Taxi Data" 
date:   2017-04-22 23:00:00
comments: true
categories: 
- R
- ggplot2
- dplyr
- fread

tags:
- R
- ggplot2
- dplyr
- fread

abstract: The rwml-R Github repo is updated with R code for exploratory data
  analysis of New York City taxi data
  from Chapter 6 of the book "Real-World Machine Learning" by 
  Henrik Brink, Joseph W. Richards, and Mark Fetherolf. Examples given include 
  reading large data files with the `fread` function from `data.table`,
  joining data frames by multiple variables with `inner_join`, and
  plotting categorical and numerical data with `ggplot2`.
---

## Abstract

{{ page.abstract }}

## Data for NYC taxi example

The data files for the examples in Chapter 6 of the book are available at 
[http://www.andresmh.com/nyctaxitrips/](http://www.andresmh.com/nyctaxitrips/).
They are compressed as a 7-Zip file archive 
(e.g. with [p7zip](http://p7zip.sourceforge.net)), so you will
need to have the `7z` command available in your path to decompress and load
the data. 
(On a mac, you can use [Homebrew](https://brew.sh) to install p7zip with 
the command `brew install p7zip`.)

## Using fread (and dplyr...again)

As in Chapter 5, the `fread` function from the 
`data.table` library is used to quickly read in a sample of the rather large
data files. It is similar to `read.table` but faster and more convenient.
The following code reads in the first 50k lines of data from one of the
trip data files and one of the fare data files. The `mutate` and `filter`
functions from `dplyr` are used to clean up the data (e.g. remove data
with unrealistic latitude and longitude values). The trip and fare data are
combined with the `inner_join` function from the `dplyr` package.

{% highlight R %}
tripFile1 <- "../data/trip_data_1.csv"
fareFile1 <- "../data/trip_fare_1.csv"
npoints <- 50000
tripData <- fread(tripFile1, nrows=npoints, stringsAsFactors = TRUE) %>%
  mutate(store_and_fwd_flag = 
           replace(store_and_fwd_flag, which(store_and_fwd_flag == ""), "N")) %>%
  filter(trip_distance > 0 & trip_time_in_secs > 0 & passenger_count > 0) %>%
  filter(pickup_longitude < -70 & pickup_longitude > -80) %>%
  filter(pickup_latitude > 0 & pickup_latitude < 41) %>%
  filter(dropoff_longitude < 0 & dropoff_latitude > 0)
tripData$store_and_fwd_flag <- factor(tripData$store_and_fwd_flag)
fareData <- fread(fareFile1, nrows=npoints, stringsAsFactors = TRUE)
dataJoined <- inner_join(tripData, fareData)
remove(fareData, tripData)
{% endhighlight %}

## Exploring the data

In [the complete code-through][chap6], plots of categorical and numerical 
features of the data are made using
`ggplot2`, including a visualization of the pickup locations in latitude and
longitude space which is shown below. With slightly less than 50,000 data 
points, we can clearly see the street layout of downtown Manhatten.
Many of the trips originate in the other boroughs of New York, too.

![The latitude/longitude of pickup locations. Note that the x-axis is flipped, compared to a regular map.](
{{ "/assets/figure6.4-1.png" | prepend: site.baseurl | prepend: site.url }}) 

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
