Running MPI jobs
================

In the HPC cluster, both OpenMPI and NVIDIA HPC-X can be used to run parallel
jobs.

They are both available as environment software modules (see :ref:`Application software through Environment modules<envswmodules>`).


To use openmpi, you need to load the relevant module file, e.g.:


::

  $ module avail
  ---------------------------------------------------- /usr/share/Modules/modulefiles -----------------------------------------------------
  dot  module-git  module-info  modules  null  use.own  

  -------------------------------------------------------- /shared/sw/modulefiles ---------------------------------------------------------
  conda-miniforge3-25.3.1  cuda-13.0   openmpi-5.0.5_gcc-12.4.0            openmpi-5.0.10_gcc-12.4.0_cuda-13.0  ucx-1.20.0_cuda-13.0  
  cuda-12.6                gcc-12.4.0  openmpi-5.0.6_gcc-12.4.0_cuda-12.6  ucx-1.17.0_cuda-12.6                 

  Key:
  modulepath  
  
  $ module load openmpi-5.0.10_gcc-12.4.0_cuda-13.0


To use hpc-x:

::

  $ module use $HPCX_LATEST/modulefiles
  $ module avail
  --------------------------------- /shared/sw/hpcx-v2.50-gcc-doca_ofed-redhat9-cuda13-x86_64/modulefiles ---------------------------------
  hpcx  hpcx-debug  hpcx-debug-ompi  hpcx-mt  hpcx-mt-ompi  hpcx-ompi  hpcx-prof  hpcx-prof-ompi  hpcx-stack  

  ---------------------------------------------------- /usr/share/Modules/modulefiles -----------------------------------------------------
  dot  module-git  module-info  modules  null  use.own  

  -------------------------------------------------------- /shared/sw/modulefiles ---------------------------------------------------------
  conda-miniforge3-25.3.1  cuda-13.0   openmpi-5.0.5_gcc-12.4.0            openmpi-5.0.10_gcc-12.4.0_cuda-13.0  ucx-1.20.0_cuda-13.0  
  cuda-12.6                gcc-12.4.0  openmpi-5.0.6_gcc-12.4.0_cuda-12.6  ucx-1.17.0_cuda-12.6                 

  Key:
  modulepath  

   $ module load hpcx


  

Compiling
---------

In the following example a simple MPI application is compiled using ``mpicc``
after having loaded
the relevant openmpi software module (in this example openmpi):


::

  [<username>@cld-ter-ui-01 ~]$ cat hello.c 
  #include <mpi.h>
  #include <stdio.h>
  #include <stddef.h>

  int main(int argc, char** argv) {
    // Initialize the MPI environment. The two arguments to MPI Init are not
    // currently used by MPI implementations, but are there in case future
    // implementations might need the arguments.
    MPI_Init(NULL, NULL);

    // Get the number of processes
    int world_size;
    MPI_Comm_size(MPI_COMM_WORLD, &world_size);

    // Get the rank of the process
    int world_rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);

    // Get the name of the processor
    char processor_name[MPI_MAX_PROCESSOR_NAME];
    int name_len;
    MPI_Get_processor_name(processor_name, &name_len);

    // Print off a hello world message
    printf("Hello world from processor %s, rank %d out of %d processors\n",
           processor_name, world_rank, world_size);

    // Finalize the MPI environment. No more MPI calls can be made after this
    MPI_Finalize();
  }
  [<username>@cld-ter-ui-01 ~]$ 

::
  
  [<username>@cld-ter-ui-01 ~]$ module load openmpi-5.0.5_gcc-12.4.0
  [<username>@cld-ter-ui-01 ~]$ mpicc -o hello hello.c



Submitting a MPI SLURM job
--------------------------

This is an example of a SLURM submit file to run a previously compiled application:

::
   
  [<username>@cld-ter-ui-01 ~]$ cat mpi.sh
  #!/bin/sh
  #SBATCH --output=/shared/home/<username>/JOB-%x.%j.out
  #SBATCH --error=/shared/home/<username>/JOB-%x.%j.err
  #SBATCH --nodes=2
  #SBATCH --ntasks-per-node=3
  #SBATCH --mail-type=ALL
  #SBATCH --mail-user=<email-address>
  
  export PMIX_MCA_psec=native
  export OMPI_MCA_mca_base_component_show_load_errors=0

  module load openmpi-5.0.5_gcc-12.4.0
  srun -l --mpi=pmix /shared/home/<username>/hello


Please note the ``module load openmpi-5.0.5_gcc-12.4.0`` directive and the
``--mpi=pmix`` option in the srun command.

Let's submit the job:

::

  [<username>@cld-ter-ui-01 ~]$ sbatch mpi.sh
  Submitted batch job 468
  [<username>@cld-ter-ui-01 ~]$ squeue 
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
               468 cpu-nodes   mpi.sh <username>  R       0:03      2 cld-ter-[01-02]
  [<username>@cld-ter-ui-01 ~]$ 


Let's see the output when the job completes:

::
  
  [<username>@cld-ter-ui-01 ~]$ cat JOB-mpi.sh.468.out 
  2: Hello world from processor cld-ter-01.cloud.pd.infn.it, rank 2 out of 6 processors
  3: Hello world from processor cld-ter-02.cloud.pd.infn.it, rank 3 out of 6 processors
  0: Hello world from processor cld-ter-01.cloud.pd.infn.it, rank 0 out of 6 processors
  5: Hello world from processor cld-ter-02.cloud.pd.infn.it, rank 5 out of 6 processors
  1: Hello world from processor cld-ter-01.cloud.pd.infn.it, rank 1 out of 6 processors
  4: Hello world from processor cld-ter-02.cloud.pd.infn.it, rank 4 out of 6 processors
  [<username>@cld-ter-ui-01 ~]$

  
