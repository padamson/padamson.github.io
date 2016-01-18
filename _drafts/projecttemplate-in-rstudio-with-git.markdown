---
layout: post
title:  "Data analysis with ProjectTemplate in RStudio with Git"
categories: R RStudio ProjectTemplate Git
---
## Abstract

[ProjectTemplate][projecttemplate] is a system for organizing and automating
data analysis in R. Here is a recipe for creating a new ProjectTemplate project in 
[RStudio][rstudio] and initializing version control with [Git][git].

## Order of Operations

I find that the following sequence is most efficient and error-proof:

1. Create ProjectTemplate project
2. Create RStudio project from directory created in Step 1
3. Create a new repo at [GitHub][github] (or [some other git hosting service][githosts])
4. Initialize git repo at the command line
5. git add, commit, and push to remote host
6. Restart RStudio

## Create ProjectTemplate project

yada yada yada

[projecttemplate]:      http://projecttemplate.net/index.html
[rstudio]:              https://www.rstudio.com
[git]:                  https://www.git-scm.com
[githosts]:             http://www.git-tower.com/blog/git-hosting-services-compared/
