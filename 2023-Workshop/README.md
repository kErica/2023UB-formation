# 202307UB-HPC Workshop
```
Contact: erica.bianco@hpcnow.com
```
The content of this folder was tested on June 2023 on the purpose to fulfill the HPC best practices Hands-on for the
WORKSHOP: Data science para biociencias (R, python, Bioinformática): 5 julio 2023 

DISCLAIMER:
The content was tested at CSUC cluster, some of the code may work elsewhere (mainly the slurm commands).

## Useful commands in HPC 

### How to login
Via command line or via web interface

```
ssh $USER@$HPC_IP 
```

#### How to move data
```
git clone https://github.com/kErica/2023UB-formation.git

scp -rp -P $PORT 2023UB-formation $USER@$HPC_IP:/home/$USER
scp -rp -P $PORT 2023UB-formation-master $USER@$HPC_IP:/home/$USER
## or
cd 2023UB-formation
sftp –oPort=$PORT $USER@$HPC_IP
mput -R *
```

### Explore the environment
```
pwd
df -h
du -hs 
ls
ls -ld .
ls -l /
```

## LMOD
### Modules based applications
```
ml
ml av
ml spider Python ## q to exit
ml spider bwa
ml spider gatk
ml Python/3.9.6-GCCcore-11.2.0 ## always with the version
which python
python -V
ml GATK/4.2.0.0-GCCcore-10.2.0-Java-11
```

##SLURM
### SLURM basic commands
```
sacct	## Displays accounting data for all jobs.
salloc	## Allocate resources for interactive use.
sbatch	## Submit a job script to a queue
scancel	## Signal jobs or job steps that are under the control of SLURM (cancel jobs or job steps)
scontrol	## View SLURM configuration and state
sinfo	## View information about SLURM nodes and partitions
sjstat	## Display statistics of jobs ( data from sinfo, squeue and scontrol).
smap	## Graphically view information about SLURM jobs, partitions, and set config. param
squeue	## View information about jobs located in the SLURM scheduling queue.
srun	## Run a parallel job
```
### How to initiate an interactive session
> **_NOTE:_**  This will work only if the user can allocate resources.
```
salloc --time 4:00:00 srun --pty bash
```
If implemented, the `interactive` command will also do the trick

> **_NOTE:_**  You can open a second terminal and have a current view of your queue jobs. `watch squeue -u $USER`

### Slurm scripts examples
#### Serial
```
#!/bin/bash
#SBATCH -J SerialJob
#SBATCH --time=01:00:00
#SBATCH --mem-per-cpu=8G
ml Application
srun serial_binary
```
#### Shared memory (OpenMP)
```
#!/bin/bash
#SBATCH -J OpenMPJob
#SBATCH --time=01:00:00
#SBATCH --mem-per-cpu=8G
#SBATCH --cpus-per-task=8
ml Application
srun openmp_binary
```
#### Distributed memory (MPI)
```
#!/bin/bash
#SBATCH -J MPIJob
#SBATCH --time=01:00:00
#SBATCH --ntasks=2
#SBATCH --mem-per-cpu=8G
ml Application
srun mpi_binary
```
#### Hybrid (MPI+OpenMP)
```
#!/bin/bash
#SBATCH -J HybridJob
#SBATCH --time=01:00:00
#SBATCH --ntasks=4
#SBATCH --mem-per-cpu=8G
#SBATCH --cpus-per-task=8
ml Application
srun hybrid_binary
```
#### Job array
```
#!/bin/bash
#SBATCH -J ArrayJob
#SBATCH --time=01:00:00
#SBATCH --mem-per-cpu=8G
#SBATCH --array=1-1000
ml Application
srun array_binary "${SLURM_ARRAY_TASK_ID}"
```
#### Serial GPU
```
#!/bin/bash
#SBATCH -J GPUJob
#SBATCH --time=01:00:00
#SBATCH --ntasks=1
#SBATCH --mem-per-cpu=8G
#SBATCH --gres=gpu:1
ml Application
srun binary_cuda
```
#### Hybrid (OpenMP + GPU)
```
#!/bin/bash
#SBATCH -J GPUJob
#SBATCH --time=01:00:00
#SBATCH --ntasks=1
#SBATCH --mem-per-cpu=8G
#SBATCH --cpus-per-task=4
#SBATCH --gres=gpu:2
ml Application
srun binary_cuda_openmp
```
#### Hybrid (MPI + GPU)
```
#!/bin/bash
#SBATCH -J GPUJob
#SBATCH --time=01:00:00
#SBATCH --ntasks=4
#SBATCH --ntasks-per-node=2
#SBATCH --mem-per-cpu=8G
#SBATCH --gres=gpu:2
ml Application
srun binary_cuda_mpi
```
#### Hybrid (OpenMP + MPI + GPU)
```
#!/bin/bash
#SBATCH -J GPUJob
#SBATCH --time=01:00:00
#pSBATCH --ntasks=4
#SBATCH --ntasks-per-node=2
#SBATCH --cpus-per-task=4
#SBATCH --mem-per-cpu=8G
#SBATCH --gres=gpu:1
ml Application
srun binary_cuda_mpi_openmp
```
