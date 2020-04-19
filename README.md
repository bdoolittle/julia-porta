julia-porta
=====

A version of the [PORTA](http://porta.zib.de/) software intended for cross-platform deployment via [julia](https://julialang.org/).

## PORTA (POlyhedron Representation Transformation Algorithm)

"PORTA is a collection of [C] routines for analyzing polytopes and polyhedra. The polyhedra are either given as the convex hull of a set of points plus (possibly) the convex cone of a set of vectors, or as a system of linear equations and inequalities." -([porta](http://porta.zib.de/)).

## Using PORTA

The GNU makefile compiles two executables, `xporta` and `valid`. The following usage documentation is taken verbatim from the porta `INFO` and `man` pages.

### `xporta`

#### `dim` 

Compute the dimension of  convex  hull  and  convex cone for a set of given points.

* `xporta -D [-pl] filename_with_suffix_'.poi'`

**Description:**

       dim  computes  the dimension of the convex hull and the convex cone of a given set of points
            by using a gaussian elimination algorithm.  It displays the computed dimension as a
            terminal message and also writes to the end of the input file. Moreover, in the case
            that the input system  is not full dimensional, dim displays the equations satisfied by
            the system.

**Options:**

       -p     Unbuffered   redirection  of  terminal  messages  into filename_'.prt'

       -l     Use  a  special  integer arithmetic allowing the integers to have arbitrary lengths.
              This arithmetic is not as efficient as the system's integer arithmetic with respect
              to time and storage requirements. Note: Output values which exceed the 32-bit integer
              storage  size  are written in hexadecimal format (hex). Such hexadecimal format can
              not be reread as input.

#### `fmel`

Projection of linear system on subspaces xi = 0.

* `xporta -F [-pcl] input_fname_with_suffix_'ieq'

**Description:**

      fmel  reads a system of linear inequalities and eliminates choosen variables.  That is,
            fmel projects the given system to the subspace given by xi = 0, for i is contained in I,
            where I is the index set of the variables that should be eliminated.  The set I and the
            elimination  order  are given  in  the input  file by a line containing   the   function
            specific   keyword "ELIMINATION_ORDER", followed by a line containing exactly dim integers,
            where dim is the dimension of the problem.  A '0' as the i-th entry of the line indicates
            that the i-th  variable  should  not  be eliminated,  that  is,  i  is not in I. An entry
            of the line indicates that the i-th variable should be eliminated in  the  j-th iteration.
            (All nonzero numbers must be different and it must be possible to put them into an order
            1,2,3,4...)  fmel writes its output into a file, whose  name is obtained by appending
            ".ieq" to the filename of the input file.

**Options:**

       -p     Unbuffered redirection of the  terminal messages into the file input_fname_with_suffix_'prt'

       -c     Generation of new inequalities without the rule of Chernikov.

       -l     Use  a  special  integer arithmetic allowing the integers to have arbitrary lengths.  This
              arithmetic is not as efficient as the system's integer arithmetic with respect to time and
              storage requirements.  Note: Output values which exceed the 32-bit integer storage  size  are
              written in hexadecimal format (hex). Such hexadecimal format can not be reread as input.

### `valid`

#### `fctp`

Checks inequalities for facet inducing property.

* `valid -C fname_with_suffix_'.ieq' fname_with_suffix_'.poi'`

**Description:**

        fctp  performs  a check whether the inequalities  given  in  the the 'poi'-input file.
              For all inequalities fctp does the following: In a first step fctp checks if the
              inequality is valid.  If  this is  not  the  case  fctp writes the points and rays
              which are not valid into a file.   In a  second  step fctp computes those valid points
              and rays which satisfy the inequality with equality and - if there are any - writes
              them into a file. For these points and rays the dimension is computed by using the
              routine  'dim'.   The  filenames  result   from   the   ieq-filename   by appending
              first the number of the corresponding inequality and then the suffix '.poi' resp.
              '.poi.poi'.


## PORTA in Julia

The julia programming language provides utilities for packaging C libraries for cross-platform deployment. The [BinaryBuilder.jl](https://github.com/JuliaPackaging/BinaryBuilder.jl) julia package reliably cross-compiles software to run on supported platforms. The end result is a julia module, `porta_jll`, containing the executables for all supported platforms and the logic to run the correct executable on that platform. 
