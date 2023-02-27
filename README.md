# 2023UB-formation
# Contact: erica.bianco@hpcnow.com
The content of this folder was tested on Feb 2023 on the purpose to fulfill the HPC best practices Hands-on for the
Ciencia de los Datos (Data Science), Aplicaciones en Biología y Medicina con Python y R
UB IL-3 course

#######################################
# List of command to run the hands-on #
#######################################

## SLURM basic commands
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

## Launch a serial job
cd Serial
sbatch serial.slm
cd ..

## Linpack MPI job
cd Linpack/
sbatch HPL_Pirineus.slm

