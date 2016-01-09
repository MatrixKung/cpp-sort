[![Build Status](https://travis-ci.org/Morwenn/cpp-sort.svg?branch=master)](https://travis-ci.org/Morwenn/cpp-sort)
[![License](http://img.shields.io/:license-mit-blue.svg)](http://doge.mit-license.org)

> *It would be nice if only one or two of the sorting methods would dominate all of the others,
> regardless of application or the computer being used. But in fact, each method has its own
> peculiar virtues. [...] Thus we find that nearly all of the algorithms deserve to be remembered,
> since there are some applications in which they turn out to be best.*
> — Donald Knuth, The Art Of Computer Programming, Volume 3

**cpp-sort** is a generic C++14 header-only sorting library. It revolves
around one main generic sorting interface and provides several small tools
to pick and/or design sorting algorithms. The library's main function,
`cppsort::sort`, is available with the following include:

```cpp
#include <cpp-sort/sort.h>
```

Using it should be pretty trivial:

```cpp
#include <array>
#include <iostream>
#include <cpp-sort/sort.h>

int main()
{
    std::array<int, 5u> arr = { 5, 8, 3, 2, 9 };
    cppsort::sort(arr);
    
    // prints 2 3 5 8 9
    for (int val: arr)
    {
        std::cout << val << ' ';
    }
}
```

**cpp-sort** actually provides a full set of sorting-related features. The most
important one is probably the concept of *sorters* and *sorter adapters*. Sorters
are function objects implementing a sorting algorithm and sorter adapters are
special class templates designed to adapt sorters and alter their behaviour in some
specific manner. The library provides sorters implementing common and not-so-common
sorting algorithms as well as some specific adapters. It also provides fixed-size
sorters and tools such as sorter facade or sorter traits, designed to craft your
own sorter.

You can read more about all the availables tools and read some tutorial about using
and extending **cpp-sort** in [the wiki](https://github.com/Morwenn/cpp-sort/wiki).

# Benchmarks

The following graph has been generated with a script found in the benchmarks
directory. It shows the time needed for one sorting algorithm to sort one million
shuffled `std::array<int, N>` of sizes 0 to 15. It compares the sorters generally
used to sort small arrays:

![shuffled int arrays](https://i.imgur.com/GaRHn9x.png)

These results were generated with MinGW g++ 5.2 with the compiler options
`-std=c++14 -O3 -march=native`. This benchmark is just an example to make the
introduction look good. You can find more commented benchmarks in the [dedicated
wiki page](https://github.com/Morwenn/cpp-sort/wiki/Benchmarks).

# Compiler support

**cpp-sort** currently works with g++ 5 and clang++ 3.8. It uses some of the most
recent (and not widely supported) C++14 features and will probably use the C++17
features once they are available. The overall goal is to make sure that the library
works with the latest g++ and clang++ versions, without going out of its way to
support older releases. 

In the future, the branches will follow the following pattern: the master branch
will remain C++14 and there will be a C++17 branch. There will be other branches
forking the C++17 one for some of the published Technical Specifications (for
example, there will likely be a branch for the parallelism TS and another one for
the concepts TS); these branches will eventually be merged in the C++17 one when
the corresponding technical specifications are merged into the current C++ working
draft (or in a C++20 branch if the specifications do not make it in time for the
C++17 release). Of course the creation of such branches will depend on compiler
support: if a feature isn't supported by either the latest g++ or clang++, I won't
use it before the following release.

# Thanks

Even though some parts of the library are [original research](https://github.com/Morwenn/cpp-sort/wiki/Original-research)
and some others correspond to custom and rather naive implementations of standard
sorting algorithms, **cpp-sort** also reuses a great deal of code from open-source
projects, often slightly altered to integrate seamlessly into the library. Here is
a list of the external resources used to create this library. I hope that the many
different licenses are compatible. If it is not the case, please contact me (or
submit an issue) and we will see what can be done about it:

* Some of the algorithms used by `insertion_sorter` and `pdq_sorter` come from
Orson Peters' [pattern-defeating quicksort](https://github.com/orlp/pdqsort). Some
parts of the benchmarks come from there as well.

* The algorithm used by `tim_sorter` comes from Goro Fuji's (gfx) [implementation
of a Timsort](https://github.com/gfx/cpp-TimSort).

* The three algorithms used by `spread_sorter` come from Steven Ross [Boost.Sort
module](http://www.boost.org/doc/libs/1_59_0/libs/sort/doc/html/index.html) with
some modifications so that they do not depend on Boost anymore.

* [`utility::as_function`](https://github.com/Morwenn/cpp-sort/wiki/Miscellaneous-utilities#as_function)
and several projection-enhanced helper algorithms come from Eric Niebler's [Range
v3](https://github.com/ericniebler/range-v3) library.

* Many projection-enhanced standard algorithms are directly adapted from their
counterparts in [libc++](http://libcxx.llvm.org/).

* The implementation of Dijkstra's smoothsort used by `smooth_sorter` has been
directly adapted from [Keith Schwarz's implementation](http://www.keithschwarz.com/interesting/code/?dir=smoothsort)
of the algorithm.

* The algorithm used by `block_sorter` has been adapted from BonzaiThePenguin's
[WikiSort](https://github.com/BonzaiThePenguin/WikiSort).

* The algorithm used by `grail_sorter` has been adapted from Mrrl's
[GrailSort](https://github.com/Mrrl/GrailSort), hence the name.

* The algorithms 17 to 22 used by `sorting_network_sorter` correspond to the ones
found by Symmetry and Evolution based Network Sort Optimization (SENSO) publihed in
*Using Symmetry and Evolutionary Search to Minimize Sorting Networks* by Valsalam
and Miikkulainen.

* The algorithms 0 to 16 used by `sorting_network_sorter` have been generated with
Perl's [`Algorithm::Networksort` module](http://search.cpan.org/~jgamble/Algorithm-Networksort-1.30/lib/Algorithm/Networksort.pm).

* Some of the optimizations used by `sorting_network_sorter` come from [this
discussion](http://stackoverflow.com/q/2786899/1364752) on StackOverflow and are
backed by the article [*Applying Sorting Networks to Synthesize Optimized Sorting
Libraries*](http://arxiv.org/abs/1505.01962).

* The LaTeX scripts used to draw the sorting networks are modified versions of
kaayy's [`sortingnetwork.tex`](https://github.com/kaayy/kaayy-s-code-sinppets),
slightly adapted to be 0-based and draw the network from top to bottom.
