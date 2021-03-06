---
title: 'Dryad'
layout: page
---

# Dryad
![dryad](/staphb_toolkit/assets/workflows/dryad/dryad_logo_250.png)  

![Latest Release](https://img.shields.io/github/v/release/k-florek/dryad)  
[![Build Status](https://travis-ci.org/k-florek/dryad.svg?branch=master)](https://travis-ci.org/k-florek/dryad)

[Dryad](https://github.com/k-florek/dryad/) - a pipeline to construct reference free core-genome or SNP phylogenetic trees for examining prokaryote relatedness in outbreaks. Dryad will perform both a reference free core-genome analysis based off of the approach outlined by [Oakeson et. al](https://www.ncbi.nlm.nih.gov/pubmed/30158193) and/or a SNP analysis using the [CFSAN-SNP](https://snp-pipeline.readthedocs.io/en/latest/readme.html) pipeline. Antimicrobial resistance genes can also be identified using [AMRFinderPlus v3.1.1](https://github.com/ncbi/amr).

## Data workflow:
![Dryad pipeline](/staphb_toolkit/assets/workflows/dryad/dryad_workflow_2.0.0.png)

## Quality Checks
Dryad performs a number of quality control steps including trimming, removing sequencing adapters, and removing contaminating phiX sequences. The workflow also performs a number of quality checks on the both the sequencing reads and assemblies.

## Additional workflow parameters
In order to tweak the versions of software used or specific workflow parameters. You can access the configuration files in the [repository](). You can use the custom configuration with the `--profile` flag.


## Quick Start:

````
$ staphb-wf dryad main <input_dir> -o <output_dir> -r <reference sequence>
````
Where `<input_dir>` is the path to an input directory containing paired-end fastq read data.
The `<output_dir>` is where the results will be written to and `<reference sequence>` is the reference sequence for SNP calling.

## Output files:
```
dryad_results
├── logs
│   ├── cleanedreads
│   ├── dryad_execution_report.html
│   ├── dryad_trace.txt
│   ├── fastqc
│   ├── quast
│   └── work
└── results
    ├── amrfinder
    ├── annotated
    ├── ar_predictions_binary.tsv
    ├── ar_predictions.tsv
    ├── assembled
    ├── cluster_report.pdf
    ├── core_gene_alignment.aln
    ├── core_genome_statistics.txt
    ├── core_genome.tree
    ├── mash
    ├── mlst.tsv
    ├── multiqc_data
    ├── multiqc_report.html
    ├── report_template.Rmd
    ├── snp_distance_matrix.tsv
    ├── snpma.fasta
    └── snp.tree
```
**ar_predictions_binary.tsv** - Presence/absence matrix of antibiotic resistance genes.  
**ar_predictions.tsv** - Antibiotic tesistance genes detected.  
**cluster_report.pdf** - Genome cluster report.  
**core_gene_alignment.aln** - Alignment of the core set of genes.  
**core_gene_statistics.txt** - Information about the number of core genes.  
**core_genome_tree.tree** - The core-genome phylogenetic tree created by the core-genome pipeline.  
**mash/{sample}.mash.txt** - Species prediction for each sample.  
**mlst.tsv** - MLST scheme predictions.  
**multiqc_report.html** - QC report.  
**report_template.Rmd** - R Markdown template for generating the cluster_report.pdf  
**snp_distance_matrix.tsv** - The SNP distances generated by the SNP pipeline.  
**snp.tree** - The SNP tree created by the SNP pipeline.  
**snpma.fasta** - The SNP alignment.  

## Docker Images
The base NextFlow configuration profiles (Docker, Singularity, and AWS) incorporate the following StaPH-B Docker Images:

| Dryad Process   | Function  | Docker Image  | Comment |
|---|---|---|---|---|
| preProcess  | Renames input read files for downstream processing | staphb/fastqc_container  | Light-weight container for quick text processing  |
| fastqc | Performs QC check on reads | staphb/fastqc:0.11.8 |
| trim | Trims reads based on quality | staphb/trimmomatic:0.39 |
| cleanreads | Remove PhiX and sequencing adapters | staphb/bbtools:38.76 |
| cfsan | CFSAN SNP Pipeline | staphb/cfsan-snp-pipeline:2.0.2 |
| snp_tree | Construct SNP tree | staphb/iqtree:1.6.7 |
| shovill | De novo assembly | staphb/shovill:1.0.4 |
| quast | QC of sequence assemblies | staphb/quast:5.0.2 |
| prokka | Annotation of assemblies | staphb/prokka:1.14.5 |
| amrfinder | Detection of AR genes | staphb/ncbi-amrfinderplus:3.1.1b |
| amrfinder_summary | Summary of detected AR genes | wslhbio/amrfinder-parser:1.0 |
| roary | Alignment of core-genomes | staphb/roary:3.12.0 |
| cg_tree | Construct CG-tree | staphb/iqtree:1.6.7 |
| multiqc | QC report | staphb/multiqc:1.8 |
| mash | Determination of Species | staphb/mash:2.1 |
| mlst | MLST typing | staphb/mlst:2.17.6-cv1 |
| render | Render the final report | staphb/cluster-report-env:1.1 |


## Version History

Latest Release: ![Latest Release](https://img.shields.io/github/v/release/k-florek/dryad)

Version [2.0.0](https://github.com/k-florek/dryad/releases/tag/v2.0.0)
1. Redesigned workflow for nextflow
2. Added phiX removal and adapter trimming with BBtools
3. Added read and assembly QC checks with FastQC, QUAST, and MultiQC
4. Added AR detection using AMRFinderPlus
5. Added MLST typing
6. Added Species id using MASH

Version [1.1](https://github.com/k-florek/dryad/releases/tag/v1.1)
1. Added the ability to run both SNP and core-genome pipelines in sucession

Version [1.0](https://github.com/k-florek/dryad/releases/tag/v1.0)
Inital Release

## Authors
[Kelsey R Florek](https://github.com/k-florek), WSLH Bioinformatics Scientist  
[Abigail Shockey](https://github.com/AbigailShockey), WSLH Bioinformatics Fellow
