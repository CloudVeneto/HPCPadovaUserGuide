Getting started
================

Who can use the INFN Padova HPC cluster
---------------------------------------

The INFN Padova HPC cluster was funded primarily by the Terabit PNRR project.

It is available to experiments and projects that have been allocated resources
following a review by the relevant bodies
(ultimately, INFN DataCloud WP3 determines where the various experiments can use their pledged resources).

Resources that have not yet been allocated, as well as pledged resources that are temporarily
unused, are available to local (Padova) users for **INFN-related scientific activities**.






.. _prerequisites:

Prerequisites for using INFN resources 
--------------------------------------

There are some specific requirements to use HPC Cluster at INFN Padova. Users who need to use INFN IT resources must:

* have a verified digital identity (LoA2) on INFN-AAI;
* declare to have read and accepted the INFN rules for the use of IT resources;
* have followed the “Corso di Sicurezza Informatica - BASE”;


Local (Padova) users must also have a local account at INFN Padova.



To have a verified digital identity, connect to https://signup.app.infn.it/ to self-register and to get an account on INFN-AAI. Documentation (unfortunately only in Italian) on this process is available here.

After this self-registration process, connect to the INFN-AAI User Portal at https://userportal.app.infn.it/ using your INFN-AAI account to complete the digital identity registration. 

In this process you will have to choose where (the INFN site) you want to be identified, and the person that will have to approve your request (if you are a Unipd student and you can’t find your Professor among the proposed list of persons, please select ‘Giannina Borgato’).

Documentation (unfortunately only in Italian) on this process is available `here <https://wiki.infn.it/cn/ccr/aai/doc/rid/istruzioni>`_ .

You will then be contacted by an INFN secretariat person for your identification.

To follow the “Corso di Sicurezza Informatica - BASE”, please use `this link <https://elearning.infn.it/course/view.php?id=105>`_ 

This course must be followed within 30 days after the registration in INFN-AAI.


When your AAI account will be approved, you need to submit a formal request for local account at INFN Padova.

You can find the procedure "Access to local IT services with the AAI account" at the following link (You can select the language from the menu in the top right corner): 

https://calcolo.pd.infn.it/

After AAI login, you can "Ask for a local account"


.. note::

   Please note the local account can be requested by people who fall into these categories:

   * INFN Employee / Contractor Padua Section
   * Employee / Contractor Department of Physics and Astronomy UniPD
   * Guest / External Collaborator / Visitor / Employee other INFN office
   * PhD student in Physics and Astronomy
   * Associate INFN Padova
   * Research fellow / Scholarship holder
   * Undergraduate
   * Intern


Apply for an account
--------------------

Once the prerequisites :ref:`prerequisites`  have been met, please contact support-hpc AT pd.infn.it for an account on the HPC cluster.
In the request please specify:

* your institution
* your role in this institution  
* a description of the experiment/use case/project for which you need such HPC
  resources
* the estimated needed resources


Login to the user inteface
--------------------------
cld-ter-ui-01.pd.infn.it is the node configured as user interface. Use ssh to connect to
this cluster with the credentials that you have been given:

::

   ssh <username>@cld-ter-ui-01.pd.infn.it

   
From this node you can submit batch jobs and or launch interactive jobs..

.. NOTE ::
   
   Please note that you can't run jobs on the user interface node.


Manage your application software
--------------------------------
If the software that you need is not installed in the cluster and it
is not available through CVMFS, 
you can install it
and make it available through a relevant environment module. Documentation is available in 
:ref:`Application software<appsw>`.

Another possible option is installing your software inside a container.

     
Submit a batch job
------------------
Once you have your compiled program and prepared its input data,
you can submit the batch job on the cluster's compute nodes.

This is a simple bash script to submit a batch job:

::

   #!/bin/sh
   #SBATCH --nodes=2 # Number of nodes
   #SBATCH --ntasks-per-node=200 # Number of tasks per node
   #SBATCH --mem=20G # Total memory requested per node
   #SBATCH --time=01:00:00 # Time limit (1 hour in this example)
   #SBATCH --partition onlycpus
   #SBATCH --qos fast
   #SBATCH --output=/shared/home/<username>/JOB.out
   #SBATCH --error=/shared/home/<username>/JOB.err
   #SBATCH --mail-type=ALL # Notify via email about all job events
   #SBATCH --mail-user=<email-address>
   srun /shared/home/<username>/<my-app>

In this script we specified:

* the needed resources for this job (--nodes, --ntasks-per-node and --mem)
* The partition and the QOS to be used for this job (--partition and --qos)  
* The maximum wallclocktime for this job (--time)
* The standard output and error (--output and --error)
* Who to be notified via e-mail and the relevant events (--mail-user and --mail-type)
* The program to be executed (<my-app>)  


.. NOTE ::
   
   Details about the available partitions and QoS are discussed in next section.

  

.. WARNING ::
   
   If you don't specify a memory directive, in this HPC cluster by default a 2 GB/core requirement is automatically set.

  
To submit the job:

::

  > sbatch [opts] <job_script>


The main user's commands of SLURM are reported in the table below.  Please
consult the man pages for more information.

+---------------------------+-----------------------------------------------------+
| Command                   | Description                                         |
+===========================+=====================================================+
| sbatch <batch script>	    | Submits a batch script to the queue                 |
+---------------------------+-----------------------------------------------------+
| squeue                    | Lists jobs in the queue                             | 
+---------------------------+-----------------------------------------------------+
| sinfo	                    | Prints queue information about nodes and partitions |
+---------------------------+-----------------------------------------------------+
| scancel <jobid>           | Cancels a job from the queue                        |
+---------------------------+-----------------------------------------------------+
| scontrol show job <jobid> | Displays detailed information for a job             |
+---------------------------+-----------------------------------------------------+
| sprio	                    | Displays information about scheduling priority      |
+---------------------------+-----------------------------------------------------+
| seff <jobid>              | Checks the efficiency for a completed job           |
+---------------------------+-----------------------------------------------------+

  
Debug your job
--------------
If you need to debug the execution of a submitted job, and for this purpose
you need to log on the relevant worker node:


::
   
   $ srun --pty --overlap --jobid <slurmjobid> bash


Run an interactive job
-----------------------

Using `salloc`, you allocate resources and spawn a shell that can be used to execute parallel
tasks.

Once the allocation is made, the salloc command starts a shell on the login node.
You can then start parallel execution on the allocated nodes with `srun`.

Example:

::
   
   [<username>@cld-ter-ui-01 ~]$ salloc --nodes 2 --ntasks-per-node=4 --time 00:10:00
   salloc: Granted job allocation 704
   salloc: Nodes cld-ter-[01-02] are ready for job
   
   bash-5.1$ srun ./hello
   Hello world from processor cld-ter-01.cloud.pd.infn.it, rank 0 out of 1 processors
   Hello world from processor cld-ter-02.cloud.pd.infn.it, rank 0 out of 1 processors
   Hello world from processor cld-ter-02.cloud.pd.infn.it, rank 0 out of 1 processors
   Hello world from processor cld-ter-01.cloud.pd.infn.it, rank 0 out of 1 processors
   Hello world from processor cld-ter-01.cloud.pd.infn.it, rank 0 out of 1 processors
   Hello world from processor cld-ter-01.cloud.pd.infn.it, rank 0 out of 1 processors
   Hello world from processor cld-ter-02.cloud.pd.infn.it, rank 0 out of 1 processors
   Hello world from processor cld-ter-02.cloud.pd.infn.it, rank 0 out of 1 processors
   bash-5.1$ 
   bash-5.1$ exit
   exit
   salloc: Relinquishing job allocation 704
   [<username>@cld-ter-ui-01 ~]$ 



You can also log (using SSH) on the node allocated using salloc, e.g.:


::
   
   [sgaravat@cld-ter-ui-01 ~]$ salloc -c 4 --ntasks=1 --mem=10G --qos=verylong
   salloc: Granted job allocation 54351
   salloc: Nodes cld-ter-03 are ready for job


You can now log on the node allocated via salloc (cld-ter-03 in this example) and run your application. Please note that
you will be "confined" to the allocated resources (4 cores and 10 GB of RAM memory in this example):

::
   
   bash-5.1$ ssh cld-ter-03
   sgaravat@cld-ter-03's password:
   Last login: Sat Mar 15 07:23:25 2025 from 192.168.60.178
   [sgaravat@cld-ter-03 ~]$ ./myapp


When you have done, use 'exit' to terminate the interactive job:


::
   
   [sgaravat@cld-ter-03 ~]$ exit
   logout
   Connection to cld-ter-03 closed.
   bash-5.1$ exit
   exit
   salloc: Relinquishing job allocation 54351
   [sgaravat@cld-ter-ui-01 ~]$ 


.. NOTE ::
   
   Please remember to close the interactive job with the command `exit` when you have
   finished, in order not to waste resources.

   
.. NOTE ::
   
   For interactive applications, another possibility is run a graphical desktop,
   as explained in :ref:`Graphical Desktop<graphdesk>`.




   

Manage containerized applications
---------------------------------
You can also manage containerized applications using SLURM.
For this cluster we provide apptainer (previously known as singularity) as
framework to run such applications.

The following is a simple example that run a container (myexample.sif) using
apptainer:

::

  #!/bin/sh
  #SBATCH --output=/shared/home/<username>/JOB.out
  #SBATCH --error=/shared/home/<username>/JOB.err
  #SBATCH --ntasks=2
  #SBATCH --mem=20G
  #SBATCH --mail-type=ALL
  #SBATCH --mail-user<email-address>
  cd $TMP_DIR
  srun apptainer run /shared/home/<username>/myexample.sif



More information about apptainer is available at the `Apptainer home page <https://apptainer.org/>`__.


More information
----------------

Please refer to the `SLURM
official documentation <https://slurm.schedmd.com/>`__ to have all the needed information
about SLURM usage.



Getting help
------------

Please contact support-hpc AT pd.infn.it for support
request.


