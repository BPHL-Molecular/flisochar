# FLISOCHAR (Florida Isolate Characterization) 
A Florida BPHL Pipeline to genomically characterize bacterial isolates

# Introduction

Flisochar owes its inspiration to FLAQ-AMR, the Florida BPHL's standard pipeline for taxonomic characterization and AMR detection.

The Flisochar's overaching goal is to improve the identification of bactorial isolates using hybrid assembly from short and long-read sequencing data. Short-read and long-read sequences can be respectively from the Illumina MiSeq system and the Oxford Nanopore Technologies. 
The pipeline is built in Nextflow, and Python is used to develop custom scripts, enabling the parse of output. It comes with singularity container to simplify installation.

# Workflow

The current worflow comprises:
1) Quality Control
2) De novo genome assembly
3) Species Identification
4) Genome Annotation
5) Detection of Antimicrobial Resistance Genes
6) Genomic Comparison

# Software Tools implemented
1. Quality control on reads: fastp, [longqc](https://github.com/yfukasawa/LongQC)
2. Three genome assemblers: canu, dragonflye, unicycler
3. Taxonomic classification progams: [Kaiju](https://github.com/bioinformatics-centre/kaiju), Kraken, Mash
4. Genome annotators: bakta, pgap, prokka 
5. Antimicrobial resistance genes marker: AMRFinderPlus
6. Average nucleotide identity (ANI): pyANI(pgap)

# Installation
At the moment, it is meant to clone this repository to your local directory.[Clone a directory](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)

## Software Requirements 
Flisochar requires Python (version 3.6 or higher with the package Pandas installed), Nextlow, Singularity (apptainer) available in your system.

### Pgap

Currenly, the installation of [pgap](https://github.com/ncbi/pgap/wiki/Quick-Start) is also required. Before installing, we recommend to create a directory path for the installation under your group
```
mkdir /*/YourGroup/UserName/repos/ncbi/pgap
```
and cd to it. Set this environment variable <PGAP_INPUT_DIR> to the created path, 
```
export PGAP_INPUT_DIR=/*/YourGroup/UserName/repos/ncbi/pgap/
```
simply to save everything on your HPC cluster. Note the slash at the end of the previous path(../pgap/) is required, so that all pgap's files are found in that directory. Download the pgap.py file as directed. Change the file into executable mode (chmod +x pgap.py). Then execute the command below on your terminal, and pagap installation will be complete. 
```
./pgap.py --update --taxcheck -D apptainer
``` 

# Resource Requirements
 
Before running flisochar, ensure that required computing resources are available.
Cores: 28, Memory: 200gb, Time ~ 2:00 hrs for one hybrid (short-read, long-read) bacterial sample 

# Running Flisochar 

Once the pipeline is available on your system (or on HiPerGator), get an interactive run by following these first steps:

```
export PGAP_INPUT_DIR=/*/YourGroup/UserName/repos/ncbi/pgap/
```

```
module load nextflow apptainer
```
The above two commands and resources may also be written in a job scheduler (sbatch or slurm script) instead.

## General Usage

```
nextflow run flisochar.nf --lreads 'Your_long-read/path/*.fastq.gz' --sreads 'Your_short-read/path/*_{1,2}.fastq.gz' --outdir << your output directory>>

```
The full usage may be accessed by executing the following command:

```
nextflow run flisochar.nf --help
```

### Example
Run the pipeline on the test dataset in your working directory using the following command:

```
nextflow run flisochar.nf --lreads 'flisochar_test_data/LRdata/*.fastq.gz' --sreads 'flisochar_test_data/SRdata/*_{1,2}.fastq.gz' --outdir flisochar_test_out 
```
### Author: Tassy J. Bazile