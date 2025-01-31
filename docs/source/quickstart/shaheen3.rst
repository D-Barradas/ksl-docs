.. sectionauthor:: Mohsin Ahmed Shaikh <mohsin.shaikh@kaust.edu.sa>
.. meta::
    :description: Shaheen3 quickstart guide
    :keywords: shaheen3, quickstart

.. _quickstart_shaheen3:

============================
Shaheen 3
============================

If you are familiar with HPC clusters and need a quick reference on the specifics of how to interact with KSL computational resources, you will find the relevant information here in a concise form. For details, please explore the other sections of the documentation starting from :ref:`available_systems`.

.. _quickstart_shaheen3_login:

Login
======
To login you need to ``ssh`` into the login node.
A ``ssh`` client should be installed on your workstation/laptop. 
For users with MacOS and Linux operating system, please open the ``Terminal`` application paste command below replacing your username for Shaheen 3.
For Windows users, you will need a application with ``ssh`` client installed within it. Please follow instruction in `this video tutorial <https://www.youtube.com/watch?v=xfAydE_0iQo&list=PLaUmtPLggqqm4tFTwhCB48gUAhI5ei2cx&index=20>`_ . When logging in to Shaheen 3, please replace the hostname with `shaheen.hpc.kaust.edu.sa` when following the steps prescribed in the tutorial.

Logging into Shaheen 3
------------------------

At present, you need to have access to Shaheen 2 login to jump to Shaheen 3. This is a two step process.
First login to Shaheen 2:

.. code-block:: bash
    :caption: SSH command to login to Shaheen 2

    ssh -X username@shaheen.hpc.kaust.edu.sa

Then login to one of the 5 login nodes of Shaheen3:

.. code-block:: bash
    :caption: SSH command to login to Shaheen 3

    ssh -X login1


.. _quickstart_shaheen3_jobscript:

==================================
Submitting your first Jobscripts
==================================

All KSL systems use SLURM for scheduling jobs for batch processing.

Shaheen 3 example jobscripts
------------------------------
On Shaheen 3 the example jobscripts below need to be submitted from ``/scratch/$USER`` directory.
This is imperative because ``/home`` directory is not mounted on compute nodes. Also ``/project`` directory is read-only on compute node.

.. note:: 
    Compute nodes on Shaheen 3 are allocated in exclusive mode. To better understand accounting on Shaheen 3, please refer to :ref:`accounting_shaheen3` section.

To submit a CPU job on Shaheen 3's AMD Genoa compute nodes, the following jobscript can be used:

.. code-block:: bash
    :caption: Change directory to ``/scratch``, copy the jobscript below and paste it in a file named e.g. ``cpu_shaheen3.slurm``

    #!/bin/bash
    #SBATCH --time=00:10:00
    #SBATCH --nodes=1
    #SBATCH --ntasks=192
    #SBATCH --hint=nomultithread

    srun -n ${SLURM_NTASKS} --hint=nomultithread /bin/hostname

The above jobscript can now be submitted using the ``sbatch`` command.

.. code-block:: bash
    
    sbatch cpu_shaheen3.slurm

To submit a GPU job on Shaheen 3's Grace Hopper compute nodes, the following jobscript can be used:

.. code-block:: bash
    :caption: Change directory to ``/scratch``, copy the jobscript below and paste it in a file named e.g. ``gpu_shaheen3.slurm``

    #!/bin/bash
    #SBATCH --time=00:10:00
    #SBATCH --gpus=4
    #SBATCH --gpus-per-node=4
    #SBATCH --ntasks=4
    #SBATCH --ntasks-per-socket=1
    #SBATCH --cpus-per-task=64
    #SBATCH --hint=nomultithread

    srun -n ${SLURM_NTASKS} --hint=nomultithread nvidia-smi

The above jobscript can now be submitted using the ``sbatch`` command.

.. code-block:: bash
    
    sbatch gpu_shaheen3.slurm


KSL has written a convenient utility called :ref:`Jobscript Generator <jobscript_generator>`. 
Use this template to create a jobscript and copy-paste it in a file in your SSH terminal on Shaheen 3 or Ibex login nodes.


If you get an error in regarding account specification, please  :email:`helpdesk <help@hpc.kaust.edu.sa>` with the your username and error and the jobscript.


