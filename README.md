quick & dirty plot
==================

`qdp` is a shell script that interfaces with [pyxplot](http://pyxplot.org.uk)
so as to generate scientific plots from the commandline. As `pyxplot`
itself, `qdp` has some defaults that should satisfy the user more often
than not. If it is not the case, there are plenty of command-line options
that control how the plots are generated. The default output consists of both
`PDF` and `PNG` files with the resulting plot.

In any case, as its name suggest, `qdp` is _quick_ and _dirty_ so if
something does not work as you expect, feel free to modify it to suit
your needs. It was originally written to enhance the scriptability of
[wasora](http://www.talador.com.ar/jeremy/wasora).

Installation
------------

Copy or symlink the file `qdp` to a binary path, such as `/usr/local/bin` or
`$HOME/bin` (provided it is in the `PATH` environment variable). Make sure
that the `pyxplot` executable can be found system-wide:

    $ which pyxplot
    /usr/bin/pyxplot

If the output of `which pyxplot` is empty, please first install 
[pyxplot](http://pyxplot.org.uk).

Usage
-----

Check the usage and available options with the `-h` option:

    $ qdp -h
    usage: ./qdp [options] [-o output] [datafile]
     no datafile indicates stdin
     if no output is given, basename is assumed equal to datafile
      if neither output nor datafile given, basename output is qdp
      either .png and .pdf will be appended to output
         --ycol "<ints>"         set y columns in datafile, default all
         --xcol <int>            set x column in datafile, default 1
         --xlabel <string>       set x axis label, default none
         --ylabel <string>       set y axis label, default none
         --xrange <range>        set x axis range, default auto
         --yrange <range>        set y axis range, default auto
         --grid                  set grid, default no
         --width <float>         set plot width in cm, default 12
         --with "<strings>"      set plotting styles, default linespoints
         --lt "<strings>"        set linetypes, default 1
         --lw "<floats>"         set linewdiths, default 1
         --plw "<floats>"        set pointlinewidths, default 1
         --ps "<float>"          set pointsizes, default 1
         --pt "<ints>"           set pointtype, default some values
         --pi "<ints>"           set pointinterval (extension to pyxplot)
         --color "<strings>"     set colors, default blue, black, green, etc.
         --ti "<strings>"        set column titles, default none
         --plottitle <string>    set plot title, default none
         --key <string>          set key location, default above
         --pre <string>          execute pyxplot commands in  <string> before plotting
         --pdf                   generate only pdf output, default both
         --png                   generate only png output, default both
         --dpi <float>           dpi to use in png output, default 127
         --header <file>         use <file> as template instead of default
         --debug                 leave the .ppl file for inspection and edition

     quotes are needed if several columns are to be plotted
     see the README.md file and the examples subdirectory


A simple call to `qdp` with `datafile` as an argument will plot all the
columns of `datafile` versus the first one with lines adding a bullet at a
random interval. The result will be written to two files named `datafile.pdf`
and `datafile.png` with dots in `datafile` replaced to dashes to avoid having
two extensions (LaTeX complains when calling `\includegraphics` otherwise).
If the first line of the datafile contains a commented (with a hash `#`) line
with the column titles, they are automatically used by qdp.

The subdirectory `examples` contains some data files that illustrate how
`qdp` can quickly build aesthetically-pleasant plots. For example, go to
the examples subdirectory and execute from the shell

    $ qdp lag.dat

If everything went fine, two figures should have been created: `lag-dat.pdf`
and `lag-dat.png`, which are ready to be included into your LaTeX article
or into your web page (respectively).

![lag](src/master/examples/lag.png)

Note that the data file `lag.dat` 
was generated using the tool [wasora](http://www.talador.com.ar/jeremy/wasora/)
from the input file `lag.was`:

    $ wasora lag.was > lag.dat

The script `qdp` can be fed through a pipe directly from wasora as

    $ wasora lag.was | qdp

In this case, as no `-o` option was given, the outputs are `qdp.pdf` and
`qdp.png`.

The default behavior can be modified using commandline arguments.
For example,

    $ qdp lag.dat -o biglag --png --dpi 600 --color "orange olivegreen" --lw "3 1" --ti "signal\$(t)\$ lag\$(t)\$" --key top

Note the quoting and the escaping of the arguments to the options
of `qdp`: each argument should consist of a single shell token, hence to
tell `qdp` which colors are to be used, the names `orange` and `olivegreen`
should be quoted to form a single token (otherwise only the `orange` would be
understood as the color to be used for column two, and `olivegreen` would be
taken as an input data file). In the same sense, the dollar sign is a special
character for the shell so it should be escape in order to actually pass
a dollar sign to `qdp` (which in turn passes it to LaTeX).


Further examples
----------------

The following invocations of `qdp` use the example data files contained in the
`examples` subdirectory.


The Lorenz attractor vs time:

    $ qdp lorenz.dat --color "brickred navyblue gray" --pi "60 71 87"


Numerical integrals of algebraic expressions:

    $ qdp algebraic.dat --ti "\$f(x)\$ \$g(x)\$ \$\\int_0^x{f(x')g(x')\,dx'}\$"


Numbers of primes less than a certain number:

    $ qdp nprimes.dat --ti "real \$n/\\ln(n)\$ \$\\int_2^n\\frac{dt}{\\ln(t)}\$" --xlabel "\$n\$" --key "bottom" --pi "1 1 1"


Distribution of thermal-hydraulic properties of an arbitrarily-powered  vertical boiling channel:

    $ qdp arbitrary-steady.dat --ti "\$q^\star(z)\$ \$u^\star(z)\$ \$h^\star(z)\$ \$\rho^\star(z)\$" --lw "2 2 1 1" --key bottom --pre "set axis x atzero" --xlabel "\$z\$" --yrange "[-1:2.1]"



qdp is copyright (c) 2013-2014 jeremy theler <jeremy@talador.com.ar>
licensed under GNU GPL version 3 or later.
wasora is free software: you are free to change and redistribute it.
There is _NO WARRANTY_, to the extent permitted by law.
