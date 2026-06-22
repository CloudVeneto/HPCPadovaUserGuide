Application software
====================
.. _appsw:

Application software in the cluster is managed through:

* CVMFS
* Environment modules

In addition, software can be managed through Conda virtual environments created by the user in the user's home directory.


Application software through CVMFS
----------------------------------
Many experiments are using CVMFS to distribute the relevant software.
In the HPC Cluster CVMFS is installed on every compute node.
By default the LHC and EGI repositories are made available. If some other
repository is needed, please contact the suppport team.
  
Application software throgh Environment modules
-----------------------------------------------
.. _envswmodules:



`Environment software modules <https://modules.readthedocs.io/en/latest>`__
can be loaded and unloaded dynamically and atomically.
Basically when you load a software module, you set the needed environment variables
(e.g. PATH, LD_LIBRARY_PATH, etc.) needed to use that software.

A software module is defined by a modulefile, which contains the information needed
to configure the shell for that application.

For the User Inteface node, you can see the available software modules using the
``module avail`` command, e.g.:

::
   
  [<username>@cld-ter-ui-01 ~]$ module avail
  -------------------------------------- /usr/share/Modules/modulefiles ---------------------------------------
  dot  module-git  module-info  modules  null  use.own  

  ------------------------------------------ /shared/sw/modulefiles -------------------------------------------
  gcc-12.4.0  openmpi-5.0.5_gcc-12.4.0  

  Key:
  modulepath  
  [<username>@cld-ter-ui-01 ~]$ 




To use one of this application, you need to load the relevant module using the
``module load <module>`` command, e.g:

::
   
  [<username>@cld-ter-ui-01 ~]$ which mpirun
  /usr/bin/which: no mpirun in (/usr/share/Modules/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin)
  [<username>@cld-ter-ui-01 ~]$ module load openmpi-5.0.5_gcc-12.4.0
  [<username>@cld-ter-ui-01 ~]$ which mpirun
  /shared/sw/openmpi-5.0.5_gcc-12.4.0/bin/mpirun
  [<username>@cld-ter-ui-01 ~]$ 




The list of the loaded modules can be retrieved using the ``module list`` command.
When you don't need anymore an application, you can unload the relevant module
using the ``module unload <module>`` command, e.g.:

::

  [<username>@cld-ter-ui-01 ~]$ module list
  Currently Loaded Modulefiles:
   1) openmpi-5.0.5_gcc-12.4.0   2) use.own  
  [<username>@cld-ter-ui-01 ~]$ module unload use.own
  [<username>@cld-ter-ui-01 ~]$ module list
  Currently Loaded Modulefiles:
   1) openmpi-5.0.5_gcc-12.4.0  
  [<username>@cld-ter-ui-01 ~]$ 



Create your own application module
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The modules in /shared/sw/modulefiles are the ones made available by the
administrators.

You can create your own module:

* by installing the relevant software in your home directory (which is
  shared between all the nodes of the cluster)
* by creating the relevant modulefile in the $HOME/privatemodules directory.
  
To use your "private" module, you need to load first the ``use.own`` module:

::

  module load use.own
  module load <proprio-modulo>


In the following example the gcc-13.4.0 module was created by the end user:

::

  [<username>@cld-ter-ui-01 ~]$ module load use.own
  [<username>@cld-ter-ui-01 ~]$ module avail
  ---------------------------------------------------------- /usr/share/Modules/modulefiles ----------------------------------------------------------
  dot  module-git  module-info  modules  null  use.own  
 
  -------------------------------------------------------------- /shared/sw/modulefiles --------------------------------------------------------------
  gcc-12.4.0  openmpi-5.0.5_gcc-12.4.0  
 
  ------------------------------------------------------- /shared/home/<username>/privatemodules --------------------------------------------------------
  gcc-13.4.0  null  
 
  Key:
  loaded  modulepath  
  [<username>@cld-ter-ui-01 ~]$ module load gcc-13.4.0
  [<username>@cld-ter-ui-01 ~]$ echo $PATH
  /shared/sw/gcc-13.4.0/bin:/shared/home/<username>/.local/bin:/shared/home/<username>/bin:/root/.local/bin:/root/bin:/usr/share/Modules/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
  [<username>@cld-ter-ui-01 ~]$ 

Use environment modules with SLURM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The following example shows a SLURM submit file where the needed software module
is loaded before executing the user payload:

::
   
  #!/bin/sh
  #SBATCH --output=/shared/home/<username>/JOB-%x.%j.out
  #SBATCH --error=/shared/home/<username>/JOB-%x.%j.err
  #SBATCH --nodes=2
  #SBATCH --ntasks-per-node=3
  #SBATCH --mail-type=ALL
  #SBATCH --mail-user=<email-address>
  module load openmpi-5.0.5_gcc-12.4.0
  srun -l --mpi=pmix /shared/home/<username>/hello

::

  

Other information
^^^^^^^^^^^^^^^^^


For more information on environment module, please see:
https://modules.readthedocs.io/en/latest.

Create a new virtual environment with Conda
-----------------------------------------------

You can use the Conda module available on the cluster to create a virtual environment and install packages in it. This is useful when you need to install custom versions of packages that must comply with the specific requirements of your software.

The first time, you'll need to load the Conda module and initialize it:

::

    [<username>@cld-ter-ui-01 ~]$ module load conda-miniforge3-25.3.1
    [<username>@cld-ter-ui-01 ~]$ conda init

::

The ``conda init`` command will add lines to the ``/shared/home/<username>/.bashrc`` file to initialize the conda environment. You need to source this file anytime you want to activate a virtual environment.


Create the virtual environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create a new virtual environment, load the Conda module and run the following commands:

::

    [<username>@cld-ter-ui-01 ~]$ module load conda-miniforge3-25.3.1
    [<username>@cld-ter-ui-01 ~]$ source .bashrc
    (base) [<username>@cld-ter-ui-01 ~]$ conda create --name myenv python=3.8

::

Replace ``myenv`` with the name you want to give to your virtual environment. You can also replace ``3.8`` with the version of Python you want to use.

The virtual environment is created in the ``/shared/home/<username>/.conda/envs/`` directory. You can list all the virtual environments you have created with the following command:

::

    [<username>@cld-ter-ui-01 ~]$ conda env list
    # conda environments:
    #
    myenv                    /shared/home/<username>/.conda/envs/myenv

::

Activate the virtual environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To activate the virtual environment, run the following commands:

::

    [<username>@cld-ter-ui-01 ~]$ module load conda-miniforge3-25.3.1
    [<username>@cld-ter-ui-01 ~]$ source .bashrc
    (base) [<username>@cld-ter-ui-01 ~]$ conda activate myenv
    (myenv) [<username>@cld-ter-ui-01 ~]$

::

The prompt will change to show the name of the virtual environment in parentheses. The python binary in the virtual environment is now the default python binary:

::

    (myenv) [<username>@cld-ter-ui-01 ~]$ which python
    /shared/home/<username>/.conda/envs/myenv/bin/python

::

Install packages in the virtual environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Now you can install packages in your virtual environment with conda. For example:

::

    (myenv) [<username>@cld-ter-ui-01 ~]$ conda install numpy scipy pandas

::

This packages will be installed in the virtual environment and will not interfere with the system packages. The path to the packages is ``/shared/home/<username>/.conda/envs/myenv/lib/python3.8/site-packages/``.


Deactivate the virtual environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To deactivate the virtual environment, run:

::

    (myenv) [<username>@cld-ter-ui-01 ~]$ conda deactivate
    (base) [<username>@cld-ter-ui-01 ~]$

::

Delete the virtual environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To delete a virtual environment, run the following command:

::

    [<username>@cld-ter-ui-01 ~]$ conda env remove --name myenv

::


Use Conda virtual environment with SLURM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following example shows a SLURM submit file where the needed software module is loaded before executing the user payload:

::
   
  #!/bin/sh
  #SBATCH --output=/shared/home/<username>/JOB-%x.%j.out
  #SBATCH --error=/shared/home/<username>/JOB-%x.%j.err
  #SBATCH --nodes=2
  #SBATCH --ntasks-per-node=3
  #SBATCH --mail-type=ALL
  #SBATCH --mail-user=<email-address>

  module load conda-miniforge3-25.3.1
  source /shared/home/<username>/.bashrc
  conda activate myenv
  srun -l /shared/home/<username>/hello
  conda deactivate

::
