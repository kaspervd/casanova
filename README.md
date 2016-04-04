CASANOVA - A *New* Way to use CASA
================================

Why casanova?
---------------------
NRAO's [CASA](https://casa.nrao.edu/docs/UserMan/UserMan.html) (Common Astronomy Software Applications) is set of C++ tools bundled together under an iPython interface and comes with its own Python installation. There are probably very good reasons to bundle a separate Python installation but sometimes this approach has its drawbacks.

Some disadvantages:
- Casapy comes with it's own (old) versions of Python, Matplotlib, etc.
- It is not possible to import Python packages which are already installed against the system Python.
- Installing third party packages is possible but devious.
- Running scripts in casapy is done using `execfile('somefile.py')`. This makes that importing scripts that are in the same directory as somefile.py does not work automatically.
- The same holds for self made modules in the same directory.
- This makes creating a more elaborate pipeline (combination of scripts) ugly.

Various other CASA users have noted these disadvantages and several workarounds exist, e.g. [drive-casa](http://drive-casa.readthedocs.org/en/latest/introduction.html) which allows for dynamic interaction with CASA from a separate Python process, [casa-python](https://github.com/radio-astro-tools/casa-python) which allows for a pip-like installation of third party packages and another project named [casa-python](https://anaconda.org/pkgw/casa-python) which packages up the CASA C++ code and Python interface in the format used by the Anaconda project. This last one makes the CASA core easily installable in Anaconda the same way that Python modules with binary code are. It comes with separate [tasks](https://github.com/pkgw/pwkit/blob/master/pwkit/environments/casa/tasks.py) and is made by [Dr. Peter K. G. Williams](https://newton.cx/~peter/about-me/). Casanova is based on his work.

The idea of casanova is to make sure that you can import CASA (toolkit and tasks) inside your normal version of Python.

Accessing the CASA toolkit
--------------------------
CASA comes with a [toolkit](https://casa.nrao.edu/docs/CasaRef/CasaRef.html) and [tasks](https://casa.nrao.edu/docs/TaskRef/TaskRef.html). In order to enable Python to import the CASA toolkit or CASA core (casac) you need to set up the dynamically-linked libraries so that they can all find each other. This is all explained in Dr. Peter K. G. Williams' [blogpost](https://newton.cx/~peter/2014/02/casa-in-python-without-casapy/) and handled in the **install_casanova** script.

In casanova, accessing the CASA toolkits (casac) comes down to:
```python
import casac
tb = casac.casac.table()
ms = casac.casac.ms()
ms.open ('vis.ms')
print ms.nrow()
ms.close()
```
Note that ms and tb are instances of the ms and table classes.

Accessing CASA tasks
--------------------
There are 137 CASA [tasks](https://casa.nrao.edu/docs/TaskRef/TaskRef.html) which you can access in casanova using:
```python
from casat import plotants
plotants.plotants(vis='myMeasurementSet.ms')
```
`casat` stands for CASA tasks. Note that the actual task function is located in a script with the same name.

Casapy uses its own installation of Python with an older version of matplotlib. In order to enable the tasks to work with the newer matplotlib installed in your own Python I had to change some files which are now in the **code** directory in this repository. I disabled the additional buttons in casapy's version of the plotter (for the differences see `plotants` in casapy and `plotants` in casanova). Also, I changed **taskinit.py** and created **\_\_init\_\_.py** in the casat directory in order to recreate a needed variable normally set when opening casapy.

The files **tasks.txt** and **dependencies.txt** contain the available CASA tasks and the files needed to execute them.

Installation
------------
These instructions are for tcsh.

1. Download the files in this repository to some directory.

2. Download and install casa (I used casa-release-4.5.2-el6) to a directory named casapy (or so). Other directories such as your casa-release-something and python_packages will also end up in this directory.

3. Find out your casapath: start the freshly installed casapy and type`import os` and `print os.environ['CASAPATH']`. Store this somewhere (you need this later in the **casanova_startup** script). I will try to make this step superfluous.

4. Download and install [patchelf](http://nixos.org/patchelf.html).

   If you don't have sudo permissions, you might want to do:
  1. Untar patchelf and go to directory.
  2. `./configure --prefix=/some/folder`
  3. `make`
  4. `make install`
  5. Set the location in your path: `set path = ($path /some/folder/bin)` Note that patchelf now only works in this instance of your shell. You can also permanently add patchelf to your path in .cshrc.

5. Modify and run the script **install_casanova**.

6. Modify the **casanova_startup** script which was automatically placed in the CASA installation directory.

7. Add `alias casanova "source /net/dedemsvaart/data2/kvdam/casa_installation/casanova_startup"` to your .cshrc file. Note that you should change the path to the **casanova_startup** script in your casa installation directory and not to the one in the directory where you stored this repository. This way everything stays in the same directory and you can later delete this repository on your computer without disabling casanova.

8. Restart your shell (or `source ~/.cshrc`) and type `casanova` or `casanova myscript.py`.

Note: I ran into problems with *libgfortran.so.3* and *libgfortran.so.3.0.0*. My other programs now preferred this fortran library over others (i.e.: version `GFORTRAN_1.4' not found (required by /usr/lib64/atlas/libtatlas.so.3)). I fixed this very bluntly by removing the libgfortran files from the \_\_casac\_\_ directory and storing them in a new folder called not_needed_libraries in the python_packages directory. I sort of hope that GFORTRAN_1.4 is newer and backwards compatible. For now, it seems to work.

Project status
--------------
I've tried quite a few of the CASA tasks but not all of them (there are 137). So it's possible that you find a bug using one of the tasks.

I ended up creating casanova for my master's research project at Leiden Observatory so after a few months I might not be a radio astronomer anymore ;-).
