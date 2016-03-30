CASANOVA - A New Way to use CASA
================================


Why casanova?
---------------------
NRAO's CASA 

Some disadvantages of this approach:
- Casapy comes with it's own (old) versions of Python, Matplotlib, etc.
- It is not possible to import Python packages which are already installed against the system Python.
- Installing third party packages is possible but devious.
- Running scripts in casapy is done using `execfile('somefile.py')`. This makes that importing scripts that are in the same directory as somefile.py does not work automatically.
- The same holds for self made modules in the same directory.
- This makes creating a more elaborate pipeline (combination of scripts) ugly.

Various other CASA users have noted these disadvantages and several workarounds exist, e.g. [drive-casa](http://drive-casa.readthedocs.org/en/latest/introduction.html) which allows for dynamic interaction with CASA from a separate Python process, [casa-python](https://github.com/radio-astro-tools/casa-python) which allows for a pip-like installation of third party packages, another project named [casa-python](https://anaconda.org/pkgw/casa-python) for anaconda which enables Python to import the CASA toolkit together with separate [tasks](https://github.com/pkgw/pwkit/blob/master/pwkit/environments/casa/tasks.py). This last [casa-python](https://anaconda.org/pkgw/casa-python) and tasks are made by [Dr. Peter K. G. Williams](https://newton.cx/~peter/about-me/). He also made a blogpost explaining how to use [CASA in Python without casapy](https://newton.cx/~peter/2014/02/casa-in-python-without-casapy/). Casanova is basically an extension of this blogposts towards also including the CASA tasks.

The idea of casanova is to make sure that you can import CASA (toolkit and tasks) inside your normal version of Python.

Casa toolkit
------------


Casa tasks
----------


Installation
------------
These instructions are for tcsh.

1. Download the files in this repository to some directory.

2. Download and install casa (I used casa-release-4.5.2-el6) to some other directory.

3. Find out your casapath: start the freshly installed casapy and type`import os` and `print os.environ['CASAPATH']`. Store this somewhere (you need this later in the **casanova_startup** script).

4. Download and install [patchelf](http://nixos.org/patchelf.html).

   If you don't have sudo permissions, you might want to do:
  1. Untar patchelf and go to directory.
  2. `./configure --prefix=/some/folder`
  3. `make`
  4. `make install`
  5. Set the location in your path: `set path = ($path /some/folder/bin)` Note that patchelf now only works in this instance of your shell. You can also permanently add patchelf to your path in .cshrc.

5. Modify and run the script **install_casanova**.

6. I ran into problems with *libgfortran.so.3* and *libgfortran.so.3.0.0*. My other programs now preferred this fortran library over others (i.e.: version `GFORTRAN_1.4' not found (required by /usr/lib64/atlas/libtatlas.so.3)). I fixed this very bluntly by removing the libgfortran files from the \_\_casac\_\_ folder and storing in a new directory called not_needed_libraries in the python_packages directory. I sort of hope that the newer libgfortran is backwards compatible. For now, it seems to work.

7. Modify the **casanova_startup** script.

8. Add `alias casanova "source /net/dedemsvaart/data2/kvdam/casa_installation/casanova_startup"` to your .cshrc file. Note that you should change the path to the **casanova_startup** script in your casa installation directory and not to the one in the directory where you stored this repository. This way everything stays in the same directory and you can later delete this repository on your computer without disabling casanova.

9. Restart your shell (or `source ~/.cshrc`) and type `casanova`.

Project status
--------------
I've tried quite a few of the CASA tasks but not all of them (there are 137). So it's possible that you find a bug using one of the tasks.

I ended up creating casanova for my master's research project at Leiden Observatory so after a few months I might not be a radio astronomer anymore ;-).

If I had more time I'd try to remove the part that creates the log files everywhere!
