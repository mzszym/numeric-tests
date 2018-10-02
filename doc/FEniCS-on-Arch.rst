Installation of FEniCS on Arch Linux
====================================

Attempted on 2nd of October 2018, FEniCS version 2018.1.0.

`FEniCS website <https://fenicsproject.org>`_

From PyPI (unsuccessful)
------------------------

.. code:: bash

   pip install fenics

This seems to install incomplete version, without dolfin not installed. `import fenics` was not working.

From AUR (not attempted)
------------------------
Not tried, because I found reports of problems, and a link to `Stable FEniCS for Arch Linux with frozen dependencies <https://github.com/sigvaldm/arch-fenics-packages>`.

From `Stable FEniCS for Arch Linux with frozen dependencies <https://github.com/sigvaldm/arch-fenics-packages>`_ (unsuccessful)
-------------------------------------------------------------------------------------------------------------------------------

`python-dolfin` does not compile, because C compiler does not find `petsc.h`, included from `petsc4py.h`.

Apart of that, problems with interactions between different versions of MPI (Intel/OpenMPI/...). I effectively disabled Intel compiler.

From conda (successful)
-----------------------

This seems successful. It requires firstly installing `conda` on Arch Linux. This can be done from AUR. 

Installation of conda from AUR
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To install conda from AUR, one has to install AUR packages `python-pycosat` and `python-conda`, in that order.

Installation of package is done as follows:

1. Visit `<https://aur.archlinux.org/>`_, and open find each package.

2. Click Download snapshot, for each package. Current download links are:

.. code::

   https://aur.archlinux.org/cgit/aur.git/snapshot/python-pycosat.tar.gz
   https://aur.archlinux.org/cgit/aur.git/snapshot/python-conda.tar.gz

3. Unpack, build, and install each package:

.. code:: bash

   tar xf python-pycosat.tar.gz
   cd python-pycosat
   makepkg -sri
   cd ..
   tar xf python-conda.tar.gz
   cd python-conda
   makepkg -sri
   cd ..

Installation requires root privileges, but building of package must be done as normal user.
 
Installation of FEniCS using conda
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is done as normal user, and installs FEniCS in virtual environment which has to be activated.

Creation of virtual environment is done by command:

.. code:: bash

   conda create -n fenicsproject -c conda-forge fenics

Virtual environment is activated by command:

.. code:: bash

   source activate fenicsproject

After that, Python code

.. code:: python

   import fenics

should work.

Note that this virtual environment is independent of system-wide packages. All additional packages have to be installed, even if they are installed in the system. For example,

.. code:: bash

   conda install matplotlib
   conda install notebook


Running tutorial
----------------

Additional difficulties are presented by the fact that tutorials on website are not up-to-date with current FEniCS version. For example, seeing results of `ft01_poisson.py` requires installation of matplotlib (see above), and changing:

.. code:: python

   interactive()

to

.. code:: python

   import matplotlib.pylab as plt
   plt.show()

`ft01_poisson.py` seems to work.

`ft02_poisson_memberane.py` does not work. It seems to require installing `mshr` component of FEniCS, which is not available in `conda`.

`ft03_heat.py` does not work, with error AttributeError:

.. code::

   Traceback (most recent call last):
     File "ft03_heat.py", line 66, in <module>
       error = np.abs(u_e.vector().array() - u.vector().array()).max()
   AttributeError: 'dolfin.cpp.la.PETScVector' object has no attribute 'array'

`ft04_heat_gaussian.py` seems to work, possibly after fixing `interactive()`.

So far so good.
