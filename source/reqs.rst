==========================================
Checking required software
==========================================
An often occuring theme in bioinformatics is installing software. Here we wil
go over some steps to help you check whether you actually have the right
software installed. There's an optional excerise on how to install ``sickle``.

Programs used in this workshop
==============================
The following programs are used in this workshop:

    - Bowtie2_
    - Velvet_
    - samtools_
    - Picard_
    - Phylosift_
    - Fastqc_
    - Sortmerna_
    - Rdp_Classifier_
    - Krona_
    - Prokka_
    - MinPath_
    - BedTools_
        
.. _Bowtie2: http://bowtie-bio.sourceforge.net/bowtie2/index.shtml
.. _Velvet: http://www.ebi.ac.uk/~zerbino/velvet/
.. _xclip: http://sourceforge.net/projects/xclip/
.. _parallel: https://www.gnu.org/software/parallel/
.. _samtools: http://samtools.sourceforge.net/
.. _CD-HIT: https://code.google.com/p/cdhit/
.. _AMOS: http://sourceforge.net/apps/mediawiki/amos/index.php?title=AMOS
.. _sickle: https://github.com/najoshi/sickle
.. _Picard: http://picard.sourceforge.net/index.shtml
.. _Ray: http://denovoassembler.sourceforge.net/
.. _Phylosift: http://phylosift.wordpress.com/
.. _Fastqc: http://www.bioinformatics.babraham.ac.uk/projects/fastqc/
.. _Sortmerna: http://bioinfo.lifl.fr/RNA/sortmerna/
.. _Rdp_Classifier: http://rdp.cme.msu.edu/
.. _Krona: http://sourceforge.net/p/krona/home/krona/
.. _Prokka: http://www.vicbioinformatics.com/software.prokka.shtml
.. _MinPath: http://omics.informatics.indiana.edu/MinPath/
.. _BedTools: http://bedtools.readthedocs.org/en/latest/

All programs are already installed, all you have to do is load the virtual
environment for this workshop. Once you are logged in to the server run::

    source /proj/g2014180/metagenomics/virtenv/bin/activate

If you would have to, you deactivate the virtual environment with the command `deactivate`, but you don't have to do that yet.

NOTE: This is a python virtual environment. The binary folder of the virtual
environment has symbolic links to all programs used in this workshop so you
should be able to run those without problems.


Using which to locate a program
===============================
An easy way to determine whether you have have a certain program installed is
by typing::

    which programname
    
where ``programname`` is the name of the program you want to use. The program
``which`` searches all directories in ``$PATH`` for the executable file
``programname`` and returns the path of the first found hit. This is exactly
what happens when you would just type ``programname`` on the command line, but
then ``programname`` is also executed. To see what your ``$PATH`` looks like,
simply ``echo`` it::
    
    echo $PATH

For more information on the ``$PATH`` variable see this link:
http://www.linfo.org/path_env_var.html.

Check all programs in one go with which
==================================================
To check whether you have all programs installed in one go, you can use ``which``.
In order to do so we will iterate over all the programs and call ``which`` on each of them.
First make a variable containing all programs separated by whitespace::

    $ req_progs="bowtie2 bowtie2-build velveth velvetg parallel samtools interleave-reads.py phylosift fastqc sortmerna prokka MinPath1.2.py bedtools"
    $ echo $req_progs
    bowtie2 bowtie2-build velveth velvetg parallel samtools interleave-reads.py phylosift fastqc sortmerna prokka MinPath1.2.py bedtools 

Now iterate over the variable ``req_progs`` and call which::

    $ for p in $req_progs; do which $p || echo $p not in PATH; done
    /proj/g2014180/metagenomics/virtenv/bin/bowtie2
    /proj/g2014180/metagenomics/virtenv/bin/bowtie2-build
    /proj/g2014180/metagenomics/virtenv/bin/velveth
    /proj/g2014180/metagenomics/virtenv/bin/velvetg
    /sw/parallel/gnuparallel/20140222/bin/parallel
    /proj/g2014180/metagenomics/virtenv/bin/samtools
    /proj/g2014180/metagenomics/virtenv/bin/interleave-reads.py
    /proj/g2014180/metagenomics/phylosift_v1.0.1/phylosift
    /sw/apps/bioinfo/fastqc/0.11.2/milou/fastqc
    /proj/g2014180/metagenomics/virtenv/bin/sortmerna
    /proj/g2014180/metagenomics/virtenv/bin/prokka
    /proj/g2014180/metagenomics/virtenv/bin/MinPath1.2.py
    /proj/g2014180/metagenomics/virtenv/bin/bedtools

In Unix-like systems a program that sucessfully completes it tasks should
return a zero exit status. For the program ``which`` that is the case if the
program is found. The ``||`` character does not mean *pipe the output onward* as
you are probably familiar with (otherwise see
http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-4.html), but checks whether the
program before it exists succesfully and executes the part behind it if not.

If any of the installed programs is missing, try to install them yourself or
ask. If you are having troubles following these examples, try to find some bash
tutorials online next time you have some time to kill. Educating yourself on
how to use the command line effectively increases your productivity immensely.

Some bash resources:

  - Excellent bash tutorial http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html
  - Blog post on pipes for NGS http://www.vincebuffalo.com/2013/08/08/the-mighty-named-pipe.html
  - Using bash and GNU parallel for NGS http://bit.ly/gwbash

(Optional excercise) Install sickle by yourself
===============================================
Follow these steps only if you want to install ``sickle`` by yourself.
Installation procedures of research software often follow the same pattern.
Download the code, *compile* it and copy the binary to a location in your
``$PATH``.  The code for sickle is on https://github.com/najoshi/sickle. I
prefer *compiling* my programs in ``~/src`` and then copying the resulting
program to my ``~/bin`` directory, which is in my ``$PATH``. This should get
you a long way::

    mkdir -p ~/src

    # Go to the source directory and clone the sickle repository
    cd ~/src
    git clone https://github.com/najoshi/sickle
    cd sickle

    # Compile the program
    make

    # Create a bin directory
    mkdir -p ~/bin
    cp sickle ~/bin
