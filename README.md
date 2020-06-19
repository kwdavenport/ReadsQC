# The Data Preprocessing workflow

## Summary

This workflow is replicate the [QA protocol](https://jgi.doe.gov/data-and-tools/bbtools/bb-tools-user-guide/data-preprocessing/) implemented at JGI for Illumina reads and use the program “rqcfilter2” from BBTools(38:44) which implements them as a pipeline. 

## Required Database

[RQCFilterData Database](http://portal.nersc.gov/dna/microbial/assembly/bushnell/RQCFilterData.tar)

It includes reference datasets of artifacts, adapters, contaminants, phiX genome, host genomes.  

## Running Workflow in Cromwell
You should run this on cori. We provide two ways to run the workflow.  
1. `SlurmCromwellShifter/`: The submit script will request a node and launch the Cromwell.  The Cromwell manages the workflow by using Shifter to run applications. 
2. `CromwellSlurmShifter/`: The Cromwell run in head node and manages the workflow by submitting each step of workflow to compute node where applications were ran by Shifter.

Description of the files in each sud-directory:
 - `.wdl` file: the WDL file for workflow definition
 - `.json` file: the example input for the workflow
 - `.conf` file: the conf file for running Cromwell.
 - `.sh` file: the shell script for running the example workflow

## The Docker image and Dockerfile can be found here

[microbiomedata/bbtools:38.44](https://hub.docker.com/r/microbiomedata/bbtools)

## Input files

1. database path, 
2. fastq (illumina paired-end interleaved fastq), 
3. output path

```
{
    "jgi_rqcfilter.database": "/global/cfs/projectdirs/m3408/aim2/database", 
    "jgi_rqcfilter.input_files": [
        "/global/cfs/cdirs/m3408/ficus/8434.3.102077.AGTTCC.fastq.gz", 
        "/global/cfs/cdirs/m3408/ficus/8434.1.102069.ACAGTG.fastq.gz", 
        "/global/cfs/cdirs/m3408/ficus/8434.3.102077.ATGTCA.fastq.gz"
    ], 
    "jgi_rqcfilter.outdir": "/global/cfs/cdirs/m3408/ficus_rqcfiltered"
}
```

## Output files

The output will have one directory named by prefix of the fastq input file and a bunch of output files, including statistical numbers, status log and a shell script to reproduce the steps etc. 

The main QC fastq output is named by prefix.anqdpht.fast.gz. 

```
|-- 8434.1.102069.ACAGTG.anqdpht.fastq.gz
|-- filterStats.txt
|-- filterStats2.txt
|-- adaptersDetected.fa
|-- reproduce.sh
|-- spikein.fq.gz
|-- status.log
|-- ...
```
