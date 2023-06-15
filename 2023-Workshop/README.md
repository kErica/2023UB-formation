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
ls
ls -ld .
ls -l /
```

#### SLURM basic commands
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
## How to initiate an interactive session
> **_NOTE:_**  This will work only if the user can allocate resources.
```
salloc --time 4:00:00 srun --pty bash
```
If implemented, the `interactive` command will also do the trick

> **_NOTE:_**  You can open a second terminal and have a current view of your queue jobs. `watch squeue -u $USER`

### Launch a serial job
```
cd 2023UB-formation-master/01-Serial
sbatch serial.slm
## check your jobs and outputs
ls -lthr
```

### Launch an array of jobs
```
sbatch --array=0-9 serial-array.slm
```

### Launch an OpenMP job
```
cd ../02-OpenMP
ls
sbatch openmp.slm
cat hello.log
```

### OpenMP, Hybrid and MPI jobs
```
cd ../03-gmx/

## OpenMP job
sbatch gmx_omp_8.slm

## hybrid job
sbatch gmx_hyb_8.slm

## MPI job
sbatch gmx_mpi_8.slm
```

