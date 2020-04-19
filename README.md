julia-porta
=====

A version of the [PORTA](http://porta.zib.de/) software intended for cross-platform deployment via [julia](https://julialang.org/).

## PORTA (POlyhedron Representation Transformation Algorithm)

"PORTA is a collection of [C] routines for analyzing polytopes and polyhedra. The polyhedra are either given as the convex hull of a set of points plus (possibly) the convex cone of a set of vectors, or as a system of linear equations and inequalities." -([porta](http://porta.zib.de/)).

## Using PORTA

The GNU makefile compiles two executables, `xporta` and `valid`. The following usage documentation is taken verbatim from the porta `INFO` and `man` pages.

### xporta

The compiled `xporta` binary exposes a number of methods documented below. Read the `man` pages for details about `.poi` and `.ieq` file formats.

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

* `xporta -F [-pcl] input_fname_with_suffix_'ieq'`

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

#### `portsort`

Sort inequality or point systems.

* `xporta -S [-s] filename_with_suffix_'.ieq'_or_'.poi'`

**Description:**

       portsort   puts  the  points or inequalities into an increasing order according to the following
                  criteria:
                            1. right  hand sides of inequalities or equations
                            2. frequency  of the values -5 .. -1, 1 .. 5
                            3. lexicographical order
                            
                  Additionally portsort formats the output. The  output  filename arises from the filename
                  of the input file by appending the suffix of the input filename once again.

**Options:**

       -s     Appending a statistical part to each line containing the number of nonzero coefficients.
       
#### `traf`

Transformation of polyhedron representations.

* `xporta -T [-poscvl] filename_with_suffix_'.ieq'_or'.poi'`

**Description:**
       traf   transforms  polyhedra between the following  two  representations:
                     - convex hull of points + convex cone of vectors (poi-representation)
                     - system of linear equations and inequalities (ieq-representation)
                     
              The direction  of  transformation  is  determined   by   the   input  filename, which  ends
              either  in  '.poi'  or in '.ieq'.  All computations are carried out in rational arithmetic  
              to  have guaranteed  correct  numerical results. Rational arithmetic uses only integer operations.
              A possible arithmetic overflow is recognized. In this case the  computations  can  be restarted
              with a special arithmetic allowing the integers to have arbitrary length. This arithmetic is not
              as efficient as the system's integer arithmetic with respect to time and storage requirements.
              The computation of  the   ieq-representation   is   performed   using  Gaussian   and Fourier-Motzkin          
              elimination.  In  the  output  file the right hand sides are 0, or  determined  by  the  smallest
              integer value  for which the coefficients of the inequality are integral.  If this is not possible
              with system integer arithmetic or if multiple  precision   integer  arithmetic  is  set,  the right
              hand sides are 0 or 1 or -1 and the values are reduced as far as possible.  If PORTA terminates
              successfully  then the resulting inequalities are all facet-defining for your polyhedron and give
              together with equations a minimal linear description of your  polyhedron.   If an 'ieq'-representation
              is given as input  and  if  0  is  not valid for the linear system, 'traf' needs a valid points.
              To give such a valid point use the function specific keyword  VALID.  The line  after VALID contains
              exactly dim rational values, where dim is the  dimension of  the space considered. 'traf' transforms
              the ieq  representation  to the poi-representation, after elimination of equations and 0-centering,
              by applying the 'poi'-to-'ieq' direction to the polar polyhedron. Hint: If you give a valid point  or
              if 0 is valid, then this vector may appear again in the resulting system, even if this vector might
              be redundant in a minimal description. (All other vectors are non-redundant.)
              
**Options:**

       -p     Unbuffered redirection of terminal messages into  file filename_'.prt'

       -o     Use  a heuristic to eliminate that variable  next,  for which the number of new inequalities is
              minimal (local criterion). If this option is set, inequalities  which are  recognized  to  be
              facet-inducing  for the finite linear system are printed into a  file as soon as they
              are identified.

       -c     Fourier-Motzkin elimination without using the rule  of Chernikov

       -s     Appends a statistical  part  to  each  line  with  the number  of coefficients

       -v     Printing a   table in the  output file which indicates strong validity

       -l     Use  a  special  integer arithmetic allowing the integers to have arbitrary lengths. This
              arithmetic is not as efficient as the system's integer arithmetic with respect to time and
              storage requirements.  Note: Output values which exceed the 32-bit integer storage size
              are written in hexadecimal format (hex). Such hexadecimal format can not be reread as input.

### valid

The compiled `valid` binary exposes the following methods. See `julia-porta/INFO` and the `man` pages and for more details.

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

#### `iespo`

Enumeration of equations  and  inequalities  that are valid for a convex cone and a convex hull.

* `valid -I [-v] fname_with_'.ieq' fname_with_'.poi'`

**Description:**

       iespo  is  a simple enumeration routine which enumerates the subset of equations  and
              inequalities in the 'ieq' input file which are valid (not necessarily facet inducing) for
              the polyhedron given by the The output is written into an 'ieq'-file, whose name is derived
              from the 'poi'-input filename.

**Options:**

       -v  Prints a table  in  the  output  file  which  indicates strong validity.
       
       
       
#### `posie`

Enumeration of points that are valid for a linear system.

* `valid -P fname_with_suffix_'.ieq' fname_with_suffix_'.poi'`

**Description:**

       posie   is a simple enumeration routine which determines the number of the points and direction
               vectors in the '.poi'-file which are valid for the linear system in the '.ieq'-file. The
               output is written into a  'poi'-file, whose name is derived from the 'ieq'-input filename.
               
#### `vint`

Enumeration of integral inner points of  a  linear  system.

* `valid -V filename_with_suffix_'.ieq'`

**Description:**

       vint   enumerates all integral points within given bounds that  are valid for a linear system.
              In the input file lower and upper bounds for each component  must be  given.  The  
              specific keywords are LOWER_BOUNDS and UPPER_BOUNDS. The line after each keyword  contains
              exactly dim  integers.  The  i-th  entry  of such a line gives the upper resp.  lower bound
              for the i-th component.  vint writes the points found into a file.  The  filename  results
              from  the  input  filename  by  replacing  the suffix '.ieq' with


## PORTA in Julia

The julia programming language provides utilities for packaging C libraries for cross-platform deployment. The [BinaryBuilder.jl](https://github.com/JuliaPackaging/BinaryBuilder.jl) julia package reliably cross-compiles software to run on supported platforms. The end result is a julia module, `porta_jll`, containing the executables for all supported platforms and the logic to run the correct executable on that platform. 
