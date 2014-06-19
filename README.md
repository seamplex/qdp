quick & dirty plot
==================

`qdp` is a shell script that interfaces with [pyxplot](http://pyxplot.org.uk)
so as to generate scientific plots from the commandline. As `pyxplot`
itself, `qdp` has some defaults that should satisfy the user more often
than not. If it is not the case, there are plenty of command-line options
that control how the plots are generated. The output consists of both
`PDF` and `PNG` files with the resulting plot.

In any case, as its name suggest, `qdp` is _quick_ and _dirty_ so if
something does not work as you expect, feel free to modify it to suit
your needs. It was originally written to enhance the scriptability of
[wasora](http://www.talador.com.ar/jeremy/wasora).

Installation
------------

Just copy or symlink the file `qdp` to a binary path, such as `/usr/local/bin` or
`$HOME/bin` (provided it is in the `PATH` environment variable).


Usage
-----

Check the usage and available options with the `-h` option:

    $ qdp -h
    usage: ./qdp [options] [-o output] [datafile]
     no datafile indicates stdin
     if no output is given, basename is assumed equal to datafile
     if neither output nor datafile given, basename output is qdp
         --xcol <int>            set x column in datafile, default 1
         --ycol <int>            set y columns in datafile, default 2
         --xlabel <string>       set x axis label, default none
         --ylabel <string>       set y axis label, default none
         --xrange <range>        set x axis range, default auto
         --yrange <range>        set y axis range, default auto
         --with <string>         set with, default linespoints
         --lt <string>           set linetype, default 1
         --lw <float>            set linewdith, default 1
         --plw <float>           set pointlinewdiths
         --ps <float>            set pointsizes
         --pt <int>              set pointtype
         --pi <int>              set pointinterval (extension to pyxplot)
         --color <string>        set colors
         --ti <string>           set data title, default none
         --plottitle <string>    set plot title, default none
         --key <string>          set key location, default top right
         --header <file>         use <file> as template instead of default

The subdirectory `examples` contains some data files that illustrate how
`qdp` can quickly build aesthetically-pleasant plots. For example, go to
the examples subdirectory and execute from the shell

    $ qdp lag.dat --ycol "2 3" --ti "\$r\$ \$y\$" --xlabel "\$t\$"

If everything went fine, two figures should have been created: `lag-dat.pdf`
and `lag-dat.png`, which are ready to be included into your LaTeX article
or into your web page (respectively). Note that the data file `lag.dat` 
was generated using the tool [wasora](http://www.talador.com.ar/jeremy/wasora/)
from the input file `lag.was`:

    $ wasora lag.was > lag.dat

The script `qdp` can be fed through a pipe directly from wasora as

    $ wasora lag.was | qdp --ycol "2 3" --ti "\$r\$ \$y\$" --xlabel "\$t\$"

In this case, as no `-o` option was given, the outputs are `qdp.pdf` and
`qdp.png`. Note the quoting and the escaping of the arguments to the options
of `qdp`: each argument should consist of a single shell token, hence to
tell `qdp` that two columns should be plotted, the numbers two and three have
to be quoted to form a single token (otherwise only the two would be
understood as a column number, and three would be taken as an input data file).
In the same sense, the dollar sign is a special character for the shell so
it should be escape in order to actually pass a dollar sign to `qdp`.


Examples
--------

The following invocations of `qdp` use the example data files contained in the
`examples` subdirectory.


The Lorenz attractor vs time:

    $ qdp lorenz.dat --ycol "2 3 4" --pt "16 17 18" --ps "0.5 0.5 0.5" --color "orange navyblue gray" --pi "60 71 87" --ti "\$x\$ \$y\$ \$z\$" --xlabel "\$t\$" --key above


Numerical integrals of algebraic expressions:

    $ qdp algebraic.dat --ycol "2 3 4" --pi "7 11 16" --xlabel "\$x\$" --ti "\$f(x)\$ \$g(x)\$ \$\\int_0^x{f(x')g(x')\,dx'}\$"


Numbers of primes less than a certain number:

    $ qdp nprimes.dat --ycol "2 3 4" --ti "real \$n/\\ln(n)\$ \$\\int_2^n\\frac{dt}{\\ln(t)}\$" --xlabel "\$n\$" --key "bottom"


Distribution of thermal-hydraulic properties of an arbitrarily-powered  vertical boiling channel:

    $ qdp arbitrary-steady.dat --ycol "2 3 4 5" --pi "61 93 74 69" --ti "\$q^\star(z)\$ \$u^\star(z)\$ \$h^\star(z)\$ \$\rho^\star(z)\$" --lw "2 2 1 1" --key bottom --pre "set axis x atzero" --xlabel "\$z\$" --yrange "[-1:2.1]"



qdp is copyright &copy; 2013-2014 jeremy theler <jeremy@talador.com.ar>
licensed under GNU GPL version 3 or later.
wasora is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
