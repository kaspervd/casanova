CASANOVA - A New Way to use CASA
================================


Why casanova?
---------------------
Let's start with some disadvantages of casapy

- Casapy comes with it's own (old) versions of Python, Matplotlib, etc.
- It is not possible to import Python packages which are already installed against the system Python.
- Installing third party packages is possible but devious.
- Running scripts in casapy is done using execfile('somefile.py'). This makes that importing scripts that are in the same directory as somefile.py does not work automatically.
- The same holds for self made modules in the same directory.
- This makes creating a more elaborate pipeline (combination of scripts) ugly.

What you ideally want:

Casa toolkit
------------


Casa tasks
----------


Installation
------------
These instructions are for tcsh.

1. Download the files in this repository to some directory.

2. Download and install casa to some directory (I used casa-release-4.5.2-el6).

3. Find out your casapath: start the freshly installed casapy and type
   `import os`
   `print os.environ['CASAPATH']`
   Store this somewhere (you need this later in the **casanova_startup** script).

4. Download and install [patchelf](http://nixos.org/patchelf.html).

   If you don't have sudo permissions, you might want to do:
  1. Untar patchelf and go to directory.
  2. `./configure --prefix=/some/folder`
  3. `make`
  4. `make install`
  5. Set the location in your path: `set path = ($path /some/folder/bin)` Note that patchelf now only works in this instance of your shell. You can also change your path in .cshrc.

5. Modify and run the script **install_casanova**

6. I ran into problems with *libgfortran.so.3* and *libgfortran.so.3.0.0*. My other programs now preferred this fortran library over others (i.e.: version `GFORTRAN_1.4' not found (required by /usr/lib64/atlas/libtatlas.so.3)). I fixed this very bluntly by removing the libgfortran files from the __casac__ folder and storing in a new directory called not_needed_libraries in the python_packages directory. I sort of hope that the newer libgfortran is backwards compatible.

7. Modify the **casanova_startup** script.

8. Add `alias casanova "source /net/dedemsvaart/data2/kvdam/casa_installation2/casanova_startup"` to .cshrc.

Project status
--------------
