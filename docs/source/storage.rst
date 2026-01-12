Storage in the HPC cluster
==========================

Three storage areas are available to the users of the cluster:

* The home directory

* A shared scratch directory

* A job scratch directory  


Users of the ENIPRED project are also made available a 20 TB disk space
available
on all
nodes at /shared/home/enipred. 




.. NOTE ::
   
   Please note that no backups are performed on these storage areas.
   



  
Home directory  
--------------

The user home directory ($HOME, /shared/home/<username>) is
shared among all nodes of the cluster.
A quota of 100 GB is assigned to each user on this storage area.


Shared scratch directory
------------------------


The second storage area (/shared-scratch/<username>) is a scratch area shared
among all nodes of the cluster. Files on this area
**whose creation/modification date is greater that 10 days are
automatically deleted**.

An overall quota of 20 TB if set on this area. At least for the time being
we don't set a quota per user, so please be polite in using this storage
area, considering the needs of other users as well.


Job scratch directory
---------------------
The third storage area is a scratch storage area on a local
storage available only on the node where the job gets executed (i.e. it is
not shared among all nodes).
Such scratch storage area,  referred by the TMP_DIR environment variable
while the job is running, is
**automatically deleted when the job completes its execution**. It is therefore
up to the job to copy the needed output files to the home directory or to
another persistent storage.

At least for the time being, there is no quota on this storage area (which is
shared between all jobs running in that node).


Guidelines
----------
We recommend to use the job scratch directory for the read/write operations
done by the job since it is implemented on NVMe disks (much faster wrt the
other storage areas).


 
The storage system technology used to implement the home directories
and the shared scratch area is not optimized for deletion operations (i.e.
the system is lazy in
releasing the storage space of deleted files). So please
take this into account, for example by not creating checkpoint data (and
deleting old checkpoints) too frequently.










