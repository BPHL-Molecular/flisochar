# FLISOCHAR(Florida Isolate Characterization) 
A Florida BPHL Pipeline to genomically characterize bacterial isolates

# Introduction

Flisochar owes its inspiration to FLAQ-AMR, the Florida BPHL's standard pipeline for taxonomic characterization and AMR detection.

The Flisochar's overaching goal is to improve the identification of bactorial isolates using hybrid assembly from short and long-read sequencing data. Short-read and long-read sequences can be respectively from the Illumina MiSeq system and the Oxford Nanopore Technologies. 
The pipeline is built in Nextflow, and Python is used to develop custom scripts, enabling the parsing through of output. It comes with singularity container to simplify installation.

# Workflow

The current worflow comprises:
1) Quality Control
2) De novo genome assembly
3) Species Identification
4) Genome Annotation
5) Detection of Antimicrobial Resistance Genes
6) Genomic Comparison

# Software Tools implemented
1. Quality control on reads: fastp, longqc
2. Three genome assemblers: canu, dragonflye, unicycler
3. Taxonomic classification progams: Kaiju, Kraken, Mash
4. Genome annotators: bakta, pgap, prokka 
5. Antimicrobial resistance genes marker: AMRFinderPlus
6. Average nucleotide identity (ANI): pyANI(pgap)

# Running Flisochar
Software requirements: You need to have Python, Nextlow, Singularity (apptainer) in your system.

Currenly the installation of [pgap](https://github.com/ncbi/pgap/wiki/Quick-Start) is also rquir.ed

Note: Before running flisochar, ensure that computing resources are available.

Resource Requirements

cores: 28, memory: 200gb, time ~ 2:00 hrs for one hybrid (short-read, long-read) bacterial sample 
 
Once the pipeline is available on your system (or on HiPerGator), as a first step, run the command below:

```
module load nextflow apptainer
```

Genaral Usage

```
nextflow run flisochar.nf --lreads '*.fastq.gz' --sreads '*_R{1,2}.fastq.gz' --outdir << your output directory>>

```
The full usage may be accessed by executing the following command:

```
nextflow run flisochar.nf --help
```



