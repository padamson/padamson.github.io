---
layout: post
title:  "Getting Started with Chapel and LAPACK"
date:   2015-10-04 22:04:00
categories: chapel
---
[ExotiMO](exotimo) is attempting to be a quantum chemistry code that
meets some fairly unique requirements:

- written in [Chapel](chapel) so has all of the [goodness that the language has to offer](chapel-overview)
- can handle arbitrary numbers and types of exotic particles 
(e.g. system with one positron, two electrons, and a quantum proton)
- can do Hartree-Fock (HF) energies (at first, but post-HF methods and geometry optimizations are planned)

The project stalled out when I wanted to diagonalize
a Hamiltonian, and I foolishly attempted to rewrite portions of BLAS and LAPACK 
in the Chapel language.

Well, just this week, I took straight to Twitter when I learned that the [Chapel](chapel) team 
released version 1.12.0 with 
[some exciting additions](chapel-changes), including a LAPACK module:

{% twitter oembed https://twitter.com/PaulEAdamson/status/649752380838420481 %}

[chapel]:         http://chapel.cray.com
[chapel-overview]:http://chapel.cray.com/overview.html
[chapel-changes]: https://raw.githubusercontent.com/chapel-lang/chapel/release/1.12/CHANGES
[exotimo]:        https://github.com/padamson/exotimo

