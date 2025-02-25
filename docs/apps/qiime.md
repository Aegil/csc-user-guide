# QIIME

## Description

QIIME (Quantitative Insights Into Microbial Ecology) is a package for comparison and analysis of microbial communities, 
primarily based on high-throughput amplicon sequencing data (such as SSU rRNA) generated on a variety of platforms, 
but also supporting analysis of other types of data (such as shotgun metagenomic data). QIIME takes users from their 
raw sequencing output through initial analyses such as OTU picking, taxonomic assignment, and construction of 
phylogenetic trees from representative sequences of OTUs, and through downstream statistical analysis, visualization, 
and production of publication-quality graphics.

On 2017 a totally rewritten version of Qiime: Qiime2 was released. The development of the original Qiime version has stopped. 
At the moment only Qiime2 is available in Puhti.


## Available

-   Puhti: qiime2-2019.7  


## Usage

In Puhti, QIIME2 can be taken in use as a _bioconda_ environment:

```text
module load bioconda
conda env list
source activate qiime2-2019.7
source tab-qiime 
```

After that you can start Qiime2 with command:
```text
qiime
```

Please check Qiime2 home page for more instructions.

Note that many Qiime tasks involve heavy computing. Thus these tasks should be executed as
batch jobs.

For example the batch job script below runs the denoising step of the
[QIIME moving pictures tutorial](https://docs.qiime2.org/2019.7/tutorials/moving-pictures/#option-1-dada2 )
as a batch job using eight cores.

```text
#!/bin/bash
#SBATCH --job-name=qiime_denoise
#SBATCH --account=<project> 
#SBATCH --time=01:00:00
#SBATCH --ntasks=1
#SBATCH --nodes=1
#SBATCH --output==qiime_out_8
#SBATCH --error=qiime_err_8
#SBATCH --cpus-per-task=8
#SBATCH --mem=16G
#SBATCH --partition=small

#set up qiime
module load bioconda
source activate qiime2-2019.7

# run task
srun qiime dada2 denoise-single \
  --i-demultiplexed-seqs demux.qza \
  --p-trim-left 0 \
  --p-trunc-len 120 \
  --o-representative-sequences rep-seqs-dada2.qza \
  --o-table table-dada2.qza \
  --o-denoising-stats stats-dada2.qza \
  --p-n-threads $SLURM_CPUS_PER_TASK
``` 

In the example above _<project>_ sould be replaced with your project name. You can use `csc-workspaces` to check your Puhti projects.
Maximum running time is set to 1 hour (`--time=01:00:00`). As QIIME2 uses threads based parallelization, the process is considered as one job that
 should be executed within one node (`--ntasks=1`, `--ntasks=1`). The job reserves eight cores `--cpus-per-task=8` that 
can use in total up to 16 GB of memory  (` --mem=16G`). Note that the nubmer of cores to be used needs to be defined in 
actual qiime command too. That is done with Megahit option `--p-n-threads`. In this case we use $SLURM_CPUS_PER_TASK 
variable that contains the _cpus-pre-task_ value ( we could as well use `--p-n-threads 8` but then we have to remember 
to change the value if the number of reserved CPUs is changed).

The job is submitted to the to the batch job system with `sbatch` command. For example, if the batch job
file is named as _qiime_job.sh_ then the submission command is: 
```text
sbatch qiime_job.sh 
```
More information about runnig batch jobs can be found from the [batch job section of the Puhti user guide](https://docs.csc.fi/#computing/running/getting-started/).



## Manual

*   [QIIME2 home page](https://qiime2.org/)




