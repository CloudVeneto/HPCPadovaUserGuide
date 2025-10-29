Overview of the INFN Padova HPC Cluster
=======================================

The resources provided by the INFN Padova HPC cluster are the following:

*  6 CPU only nodes cld-ter-01..06: each node has 2 AMD EPYC 9654 processors
   (192 Cores, 384 threads in total) and 1.5 TB of RAM
*  5 CPU-GPU nodes cld-ter-gpu-01..05: each node has 2 AMD EPYC 9654 processors
   (192 Cores, 384 threads in total), 1.5 TB of RAM and 4 NVIDIA H100 80GB HBM3 GPUs

These nodes are interconnected through a high bandwith, low latency Infiniband network.

The cluster also integrates two servers owned by the the "Quantum Matter and
Information" group:

* a node (cld-dfa-gpu-06) with 2 Intel Xeon Platinum 8452Y processors
  (72 cores, 144 threads), 1.5 TB
  of RAM and 1 NVIDIA L40S GPU.
* a node (btc-dfa-gpu-02) with 2 Intel Xeon Gold 6534 processors
  (16 cores, 32 threads), 1.5 TB
  of RAM and 2 NVIDIA L40S GPU.

Such resources are managed through the SLURM resource manager.


Available SLURM partitions
--------------------------
.. _Partitions:



A Slurm partition exposes a set of resources.
Coupled with each partition there are a list of QOS, that specifies 
constraints (e.g. on the number of maximum usable resources, on the job time
limit, etc.) and other characteristics (e.g. priority).

When you submit a job, you must specify the partition and the QOS to be used (unless
you are fine with the default values)



In the INFN Padova HPC cluster there are:

* partitions available to all users.
* partitions reserved to specific projects (therefore usable only by the relevant users).
* partitions available to all users that allow to use opportunistically resources allocated to specific projects, when they are available.
  **Please note that jobs submitted to these partitions will be preempted (requeued) if jobs submitted to higher priority partitions require those resources.**


The following table describes the available partitions:




.. list-table:: 

   * - **Partition**
     - **Usable by**
     - **To be used for**
     - **Priority**
     - **Notes**
   * - **onlycpus**
     - all users
     - | jobs which don't
       | require GPUs
     - 20
     - default partition
   * - **gpus**
     - all users
     - | jobs which need
       | one or more H100 GPUs
     - 20
     - 
   * - **enipiml**
     - | users of the
       | ENIPIML project
     - | both gpus (up to 4 H100
       | GPUS) and only cpus jobs
     - 20
     - 
   * - **enipred**
     - | users of the
       | ENIPRED project
     - | both gpus (up to 4 H100
       | GPUS) and only cpus jobs
     - 20
     - 
   * - **sparch**
     - | users of the
       | SPARCH project
     - | both gpus (up to 4 H100
       | GPUS) and only cpus jobs
     - 20
     - 
   * - **qst**
     - | users of the
       | QST project
     - | both gpus (up to 2 L40S) and
       | only cpus jobs
     - 20
     - 
   * - **onlycpus-opp**
     - all users
     - | jobs which don't require 
       | GPUs and which can
       | manage preemption
     - 10  
     - | Jobs submitted to this partition will
       | be cancelled and requeued if jobs
       | submitted to higher priority partitions
       | need those resources
   * - **gpus-opp**
     - all users
     - | jobs which require one or
       | more GPUs (H100 and L40S)
       | and which can manage
       | preemption
     - 15 
     - | Jobs submitted to this partition will
       | be cancelled and requeued if jobs
       | submitted to higher priority partitions
       | need those resources





  
.. NOTE ::

   The `gpus` and `gpus-opp` partitions can be used only if your job needs one or more GPU accelerators.  


Available QoS
-------------


   
The following table summarizes which are the available QOS:


.. list-table::
   
   * - **QoS**
     - **MaxWallClockTime**
     - | **Max running**
       | **jobs per user**  
     - **Max usable resources**
     - **Priority**
     - **Notes**
   * - **normal**
     - 24 hours
     -  
     - | 60 cpu-threads, 240 GB of RAM memory
       | 4 GPUs NVIDIA H100 80GB HBM3
     - 60  
     - Default QoS 
   * - **fast**
     - 2 hours
     -   
     - | 80 cpu-threads, 320 GB of RAM memory
       | 4 GPUs NVIDIA H100 80GB HBM3
     - 100  
     - 
   * - **long**
     - 3 days
     - 6  
     - | 40 cpu-threads, 160 GB of RAM memory
       | 2 GPUs NVIDIA H100 80GB HBM3
     - 40  
     - 
   * - **verylong**
     - 7 days
     - 2  
     - | 20 cpu-threads, 80 GB of RAM memory
       | 2 GPUs NVIDIA H100 80GB HBM3
     - 20  
     - 

     

Moreover there are some QoS implemented at partition level, i.e. they are
automatcally enforced for jobs submitted to the relevant partitions:


     

.. list-table:: 

   * - **QoS**
     - **MaxWallClockTime**
     - | **Max running**
       | **jobs per user**  
     - **Max usable resources**
     - **Priority**
     - **Notes**
   * - **enipiml**
     - 30 days
     -   
     - | 384 cpu-threads, 1500 GB of RAM memory
       | 4 GPUs NVIDIA H100 80GB HBM3
     - 60  
     - Applied to jobs submitted to the enipiml partition
   * - **enipred**
     - 30 days
     -   
     - | 384 cpu-threads, 1500 GB of RAM memory
       | 4 GPUs NVIDIA H100 80GB HBM3
     - 60  
     - Applied to jobs submitted to the enipred partition
   * - **sparch**
     - 30 days
     -   
     - | 384 cpu-threads, 1500 GB of RAM memory
       | 4 GPUs NVIDIA H100 80GB HBM3
     - 60  
     - Applied to jobs submitted to the sparch partition
   * - **qst**
     - 30 days
     -   
     - | 144 cpu-threads, 1500 GB of RAM memory
       | 2 GPUs NVIDIA L40S
     - 60  
     - Applied to jobs submitted to the qst partition


