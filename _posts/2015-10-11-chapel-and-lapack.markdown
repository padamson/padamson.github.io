---
layout: post
title:  "Some notes on getting started with Chapel's new LAPACK module on a mac"
date:   2015-10-11 11:24:00
categories: chapel exotimo lapack
---
## Abstract
The latest version of Chapel (version 1.12.0) includes a LAPACK module. 
To get started, I compiled Chapel, BLAS, and LAPACK, and I ran the 
example code shipped with Chapel. Here are some notes.[^1]

[^1]: This is not meant to be step-by-step instructions. I don't outline every step in detail, and I assume the reader has previously compiled and used Chapel on his system and already has things like Homebrew and the Xcode command-line tools.

## Software versions referenced:
- [Chapel][chapel] version 1.12.0
- [BLAS][blas] version 3.5.0
- [LAPACK][lapack] version 3.5.0
- gcc (including gfortran) version 5.2.0 (installed via Homebrew)
- Xcode version 7.0
- Apple LLVM version 7.0.0

## Motivation
I've started the [ExotiMO][exotimo] project to write a quantum chemistry code that
meets some fairly unique requirements:

- written in [Chapel][chapel] so it has all of the [goodness that the language has to offer][chapel-overview]
- can handle arbitrary numbers and types of exotic particles 
(*e.g.* system with one positron, two electrons, and a quantum proton)
- can do Hartree-Fock (HF) energies (at first, but post-HF methods and geometry optimizations are planned)

The project stalled out when I wanted to diagonalize
a Hamiltonian, and I foolishly attempted to rewrite portions of BLAS and LAPACK 
in the Chapel language. So, when the [Chapel][chapel] team 
released version 1.12.0 with 
[some exciting additions][chapel-changes], including a LAPACK module, I was anxious to
try it out.

## Getting BLAS, LAPACK, and Chapel
LAPACK will require `gfortran` and BLAS to compile, and BLAS will require `gfortran`. `gfortran` can be 
gotten easily along with `gcc` using [Homebrew][homebrew], and they will install to 
`/usr/local/Cellar/gcc/5.2.0/bin` (as of this writing) as `gcc-5` and `gfortran` with
symlinks in `/usr/local/bin`.  Run `brew info gcc` to confirm for yourself. When installing `gcc` with
`brew` (`brew install gcc`), you may run into a problem with the linking of packages 
`mpfr`, `gmp`, and `libmpc`. This is easily solved with the following commands:
{% highlight bash %}
brew unlink gmp && brew unlink mpfr && brew unlink libmpc && brew link gmp mpfr libmpc
brew reinstall gcc
{% endhighlight %}

Both [BLAS][blas] and [LAPACK][lapack] use `make.inc` files for system-specific configurations. The 
only thing you might want to change in the BLAS `make.inc` that is already in the root directory
of the distribution is the `PLAT` variable to append an identifier
to the library filename. (I used `_DARWIN`.) Then just run `make`, and you will have a `blas_DARWIN.a`
file.

For the LAPACK `make.inc` file, I copied the file from `INSTALL/make.inc.gfortran`, and only two changes
were necessary. Since I'm using the `gfortran` from Homebrew, I pointed `CC` to the gcc from there, too 
(`CC = gcc-5`). Also, you want to make sure you are using the BLAS that you just compiled by 
pointing `BLASLIB` to the appropriate file. (In my case, `BLASLIB = $(HOME)/BLAS/blas_DARWIN.a`.)
The Chapel LAPACK module actually uses [LAPACKE][lapacke], the C interface to LAPACK, so the command
to compile is actually `make lapackelib`. You can check the LAPACKE install by running the 
example shipped with the code (by running `make lapacke_example`).

(Updated 17 Jan 2016)<del>Although t</del><span style="color:red">T</span>he 
[Chapel install page][chapel-install] mentions [Homebrew][homebrew] as an option for 
installing a single-locale (shared memory) build of Chapel<span style="color:red">.
Since I am attempting it on the same day as the 1.12.0 release and there is some lag on the order of 24 hours
for the Homebrew formulas to be updated</span>, my Homebrew would install version 1.11.0, 
not the latest one with the LAPACK module.
When installing Chapel from source, I've initially decided to use the default options in the
Makefile using the Apple LLVM compiler that ships with Xcode command-line tools.

## Running the example
Chapel ships with an example program (located at `examples/primers/LAPACKlib.chpl`)
that uses the LAPACK `gesv` procedure to solve
$$ A \times X = B $$ for $$ X $$ given both $$ A $$ and $$ B $$, where $$ A $$ is a square matrix.
It also includes a little bit of verification and validation by 

- computing $$ B $$ from random $$ A $$ and $$ X $$ matrices before passing $$ A $$ and $$ B $$ to `gesv`
- comparing the computed $$ X $$ matrix from `gesv` to the $$ X $$ matrix that was previously generated

A minimal Makefile to compile `LAPACKlib.chpl` looks something like this:

{% highlight Makefile %}
LAPACK=$(HOME)/lapack-3.5.0
BLAS=$(HOME)/BLAS
LAPACKE=$(LAPACK)/lapacke/include
LIBGFORTRAN=/usr/local/Cellar/gcc/5.2.0/lib/gcc/5

all:
        chpl -I$(LAPACKE) \
          -L$(LIBGFORTRAN) -lgfortran \
          -L$(LAPACK) -llapacke -llapack \
          -L$(BLAS) -lblas \
          LAPACKlib.chpl

FORCE:
{% endhighlight %}

Success looks something like this:
{% highlight text %}
Matrix A:
0.000711237 0.770936
0.708429 0.847064

Matrix X:
0.000711237
0.770936

Matrix B:
0.594342
0.653536

gesv result for X:
0.000711237
0.770936

SUCCESS
{% endhighlight %}

[chapel]:         http://chapel.cray.com
[chapel-overview]:http://chapel.cray.com/overview.html
[chapel-changes]: https://raw.githubusercontent.com/chapel-lang/chapel/release/1.12/CHANGES
[chapel-install]: http://chapel.cray.com/install.html
[exotimo]:        https://github.com/padamson/exotimo
[seamless]:       https://github.com/padamson/seamless
[homebrew]:       http://brew.sh
[blas]:           http://www.netlib.org/blas/
[lapack]:         http://www.netlib.org/lapack/
[lapacke]:        http://www.netlib.org/lapack/lapacke.html
