---
layout: post
title:  "Getting Started with Chapel and LAPACK on a Mac"
date:   2015-10-04 22:04:00
categories: chapel seamless
---
[ExotiMO](exotimo) is attempting to be a quantum chemistry code that
meets some fairly unique requirements:

- written in [Chapel](chapel) so has all of the [goodness that the language has to offer](chapel-overview)
- can handle arbitrary numbers and types of exotic particles 
(e.g. system with one positron, two electrons, and a quantum proton)
- can do Hartree-Fock (HF) energies (at first, but post-HF methods and geometry optimizations are planned)

The project stalled out when I wanted to diagonalize
a Hamiltonian, and I foolishly attempted to rewrite portions of BLAS and LAPACK 
in the Chapel language. So, when the [Chapel](chapel) team 
released version 1.12.0 with 
[some exciting additions](chapel-changes), including a LAPACK module, I was anxious to
try it out.

Before diving into integration with ExotiMO, I thought a simple test-drive would be appropriate.
For this, I figured I would extend one of my other projects, [seamless](seamless), by adding a 
simple linear algebra problem to the suite of examples.

For those unfamiliar with [seamless](seamless), it is a tool for science code development in which
requirements, documentation, source code, and a test suite are all contained in a LaTeX document. 
It is meant to encourage good software engineering practices, including a test-driven development approach. 

# Installing Chapel
LAPACK will require `gfortran` and BLAS. `gfortran` can be gotten along with `gcc` 
version 5.2.0 with Homebrew. They install to `/usr/local/Cellar/gcc/5.2.0/bin` as `gcc-5` and `gfortran`. 
Run `brew info gcc` to confirm. 

I decided to 
compile Chapel, BLAS, and LAPACK all with `gcc-5` and the `gfortran` that come with it through Homebrew.

First, I installed gcc with Homebrew:


Although the [Chapel install page](chapel-install) mentions [Homebrew](http://brew.sh/) as an option for 
installing a single-locale (shared memory) build of Chapel, I chose to download a copy from GitHub so that
I could 

[chapel]:         http://chapel.cray.com
[chapel-overview]:http://chapel.cray.com/overview.html
[chapel-changes]: https://raw.githubusercontent.com/chapel-lang/chapel/release/1.12/CHANGES
[chapel-install]: http://chapel.cray.com/install.html
[exotimo]:        https://github.com/padamson/exotimo
[seamless]:       https://github.com/padamson/seamless

