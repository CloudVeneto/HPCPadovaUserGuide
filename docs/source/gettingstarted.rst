Getting started
================

Apply for an account
^^^^^^^^^^^^^^^^^^^^
TBC

Login to the user inteface
^^^^^^^^^^^^^^^^^^^^^^^^^^
cld-ter-ui-01.pd.infn.it is the node configured as user interface. Use ssh to connect to
this cluster with the credentials that you have been given:

::

   ssh <username>cld-ter-ui-01.pd.infn.it

   
From this node you can submit batch jobs and or launch interactive jobs..

.. NOTE ::
   
   Please note that you can't run jobs on the user interface node.


Manage your application software
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In this cluster, applications software is managed through environment modules.
If the software that you need is not available yet in the cluster, you can install it
and write the relevant environment module. Documentation is available in 
:ref:`Application software<appsw>`.

Submit a batch job
^^^^^^^^^^^^^^^^^^
Once you have your compiled program and prepared its input data,
you can submit the batch job on the cluster's compute nodes.

This is a simple bash script to submit a batch job:

::

   #!/bin/sh
   #SBATCH --nodes=2 # Number of nodes
   #SBATCH --ntasks-per-node=200 # Number of tasks per node
   #SBATCH --mem=20G # Total memory requested per node
   #SBATCH --time=01:00:00 # Time limit (1 hour in this example)
   #SBATCH --partition cpu-nodes
   #SBATCH --output=/shared/home/sgaravat/JOB.out
   #SBATCH --error=/shared/home/sgaravat/JOB.err
   #SBATCH --mail-type=ALL # Notify via email about all job events
   #SBATCH --mail-user=massimo.sgaravatto@pd.infn.it
   srun /shared/home/sgaravat/<my-app>

In this script we specified:

* the needed resources for this job (--nodes, --ntasks-per-node and --mem)
* The partition to be used for this job (--partition)
* The maximum wallclocktime for this job (--time)
* The standard output and error (--output and --error)
* Who to be notified via e-mail and the relevant events (--mail-user and --mail-type)
* The program to be executed (<my-app>)  

To submit the job:

::

  > sbatch [opts] <job_script>



Launch an interactive job
^^^^^^^^^^^^^^^^^^^^^^^^^

Using `salloc`, you allocate resources and spawn a shell that can be used to execute parallel
tasks.

Once the allocation is made, the salloc command starts a shell on the login node.
You can then start parallel execution on the allocated nodes with `srun`.

Example:

::
   
   [sgaravat@cld-ter-ui-01 ~]$ salloc --nodes 2 --ntasks-per-node=4 --time 00:10:00
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
   [sgaravat@cld-ter-ui-01 ~]$ 


.. NOTE ::
   
   Please remember to close the interactive job with the command `exit` when you have
   finished, in order not to waste resources.



More information
^^^^^^^^^^^^^^^^^^^

Please refer to the `SLURM
official documentation <https://slurm.schedmd.com/>`__ to have all the needed information
about SLURM usage.



Getting help
^^^^^^^^^^^^
TBC 
