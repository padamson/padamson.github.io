---
layout: post
title:  "Syntax highlighting of code within a LaTeX document in Vim"
date:   2016-02-27 10:00:00
comments: true
categories: 
- chapel
- seamless
tags:
- chapel
- vim
- seamless
abstract: I describe the steps necessary to enable syntax highlighting of 
  code snippets within a LaTeX document when editing in Vim. As an example,
  I show how to enable syntax highlighting for Chapel code. This is useful
  for developing Chapel code with the seamless framework.
---

## Abstract

{{ page.abstract }}

## Why enable syntax highlighting of code snippets within a LaTeX document in Vim?

If you are using Vim to edit a LaTeX document that contains code snippets,
then you will find it useful
to turn on syntax highlighting of both the LaTeX code and the *other* code.
For example, if you are developing Chapel code with the 
[seamless](https://github.com/padamson/seamless) framework, then you will
have Chapel source code and test code inside 'chapel' environments within your 
seamless LaTeX document. These 'chapel' environments will occur within 'chapelexample', 'chapelsource',
and 'chapelhelper' environments and are delimited by an opening `\begin{chapel}` tag and a
closing `\end{chapel}` tag.  The following steps will make it so that everything inside
those tags in your LaTeX documents will be highlighted with the rules defined in your
Vim install for files with a 'chpl' extension.

## The steps

1. Enable syntax highlighting of the non-LaTeX code in Vim using the appropriate plugin. For
example, a plugin for syntax highlighting of Chapel source can be found in the
`etc/vim` directory of the Chapel source. Syntax highlighting of other common languages 
(e.g. C++) is enabled by simply putting `syntax on` in your `~/.vimrc` file.

2. Install the [ingo-library](http://www.vim.org/scripts/script.php?script_id=4433) plugin for Vim

3. Install the [SyntaxRange](http://www.vim.org/scripts/script.php?script_id=4168) plugin for Vim

4. Put the following code in `~/.vim/after/syntax/tex/Syntax.Include.vim`:

```
call SyntaxRange#Include('\\begin{lang-env}', '\\end{lang-env}', 'lang-ext')
```

Where `lang-env` is the argument that you will use for the `\begin` and `\end` LaTeX keywords to
specify that you are including your non-LaTeX code. For example, for Chapel code, the line in 
`~/.vim/after/syntax/tex/Syntax.Include.vim` would be:

```
call SyntaxRange#Include('\\begin{chapel}', '\\end{chapel}', 'chpl')
```

To properly format the code snippet in the output of your LaTeX document, you will need to include
the 'listings' package in your LaTeX document with `\usepackage{listings}` and define the language syntax 
by inputting the appropriate 'language_listing.tex' file (e.g. `\input{chapel-listing.tex}`). Download an 
example listings file for Chapel: [chapel-listing.tex][1].

[1]:{{ site.url }}/download/chapel-listing.tex
