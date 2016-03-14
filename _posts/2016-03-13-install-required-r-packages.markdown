---
layout: post
title:  "Install all required R packages on your Shiny server"
date:   2016-03-13 18:00:00
comments: true
categories: 
- R
- Shiny
tags:
- R
- Shiny
- bash
- scripting
abstract: I give a walkthrough of a bash script that installs 
    all of the R packages required by an R program (e.g., Shiny app,
    R file, R markdown file). This is useful for speeding up the workflow
    of adding a new Shiny app to a server.
---

## Abstract

{{ page.abstract }}

## Why do we need a script?

As explained in [Dean Attali's excellent post on how to setup an RStudio 
and Shiny server](http://deanattali.com/2015/05/09/setup-rstudio-shiny-server-digital-ocean/), 
you can install an R package (for example 'mypackage') for 
everyone on a server at the command line with:[^1]

{% highlight bash %}
sudo su - -c "R -q -e \"install.packages('mypackage', repos='http://cran.rstudio.com/')\""
{% endhighlight %}

[^1]: Note that I added the `-q` argument to suppress printing of the startup message.

Repeating this long command once or twice is fine, but if you have an app that
requires several R packages such as:

{% highlight R %}
# Dean Attali
# November 21 2014

# This is the server portion of a shiny app shows cancer data in the United
# States

source("helpers.R")  # have the helper functions avaiable

library(shiny)
library(magrittr)
library(plyr)
library(dplyr)
library(tidyr)
library(ggplot2)
library(shinyjs)

# Get the raw data
cDatRaw <- getData()
...
{% endhighlight %}

then you want to automate the task of installing missing R packages.

## The bash script

For the impatient among you, here is the entire script:

{% gist b2eb520ac677c6809faf %}

The one argument passed to the script is the location of the R file that 
contains `library(mypackage)` or
`require(mypackage)` commands (each on a separate line). The script will 
determine whether or not each required
package is installed, and if it is not, the package will be
installed from CRAN.

The first few lines simply check to make sure that a single argument
is provided. If not, a usage reminder is echoed and the script exits. 
Otherwise, the argument is saved so that it is available in the
variable `$file`.

{% highlight bash %}
if [ $# -ne 1 ]; then
  echo $0: usage: installAllRPackages.bash file
  exit 1
fi

file=$1
{% endhighlight %}

The next set of lines creates three temporary files in the '~/tmp' directory
(it must exist!) and sets a trap to delete them on exit.

{% highlight bash %}
tempfile() {
  tempprefix=$(basename "$0")
  mktemp ~/tmp/${tempprefix}.XXXXXX
}

TMP1=$(tempfile)
TMP2=$(tempfile)
TMP3=$(tempfile)

trap 'rm -f $TMP1 $TMP2 $TMP3' EXIT
{% endhighlight %}


The next two lines employ `grep` to search the R file for `library` and
`require` commands, placing any lines containing the commands in the temporary
file `$TMP1`.

{% highlight bash %}
grep library $file >> $TMP1
grep require $file >> $TMP1
{% endhighlight %}

Then, `awk` is used to extract the name of each package by looking
inside the parenthesis on each line of the `$TMP1` file.
The end result is a `$TMP2` file that contains the name of an R
package on each line.

{% highlight bash %}
awk -F "[()]" '{ for (i=2; i<NF; i+=2) print $i }' $TMP1 >> $TMP2
{% endhighlight %}

The real meat of the script is in the final `while` loop. In each
iteration of the loop, a package name is extracted from `$TMP2` and
stored in the variable `$p`. We will reuse the `$TMP3` file, so
we empty it with the `truncate` command at the start of each loop.
Also inside the loop, the command
{% highlight bash %}
sudo su - -c "R -q -e \"is.element('$p', installed.packages()[,1])\"" >> $TMP3 
{% endhighlight %}
calls R to check to see if the package name is an element of the array
returned from the `installed.packages()` command. The result is stored in
the file `$TMP3`. If the R command returns `[1] TRUE`, then on to the
next package. Otherwise, the package is installed.
The entire `while` loop:

{% highlight bash %}
while read p; do
  truncate -s 0 $TMP3
  sudo su - -c "R -q -e \"is.element('$p', installed.packages()[,1])\"" >> $TMP3 
  if grep -Fxq "[1] TRUE" $TMP3
  then
    echo "$p package already installed"
  else
    echo "installing $p package"
    sudo su - -c "R -q -e \"install.packages('$p', repos='http://cran.rstudio.com/')\""
  fi
done <$TMP2
{% endhighlight %}
