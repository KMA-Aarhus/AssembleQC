# AssembleQC
Input: Paired reads.    
Output:  
- QC report. 
- Taxonomic analysis. 
- Genome Assembly. 
- Genome annotation. 
## Installation. 
Requires conda and snakemake to run.  
https://docs.conda.io/en/latest/miniconda.html. 
https://snakemake.readthedocs.io/en/stable/. 
### Install with git
```
git clone https://github.com/KMA-Aarhus/AssembleQC.git
```
## How to run
Set up an alias for snakemake to run on slurm:
```
alias snakeslurm='mkdir -p logs/old; mv logs/*.{err,out} logs/old 2> /dev/null; snakemake --profile configs/slurm --use-conda --conda-frontend mamba'
```
Navigate to AssemblyQC directory where the snakefile is.  
Currently, all fastq files to analyze must be placed in the same directory. Alternatively, additional analysis can be started.
### To run 
```
snakeslurm --config raw_uploads_dir="path_to_fastq_files" trimmed="n"
Replace "path_to_fastq_files" with the path to the raw data.
Replace trimmed="n" with trimmed="y" if the input files are already trimmed.

Additional optional options:
out_base # defines the main output directory. Default="output"
sample_reads # Sets the downsampling amount. Should be changed for very small or very large genomes. Default=5000000

option # option="UMI" if read data contains UMIs. 
kraken2_db # Location of the kraken2 database. Default="/project/ClinicalMicrobio/faststorage/database/kraken2_20210423"
```

### Output
Output can be found in the specified output directory. Output contains:
* A multi-qc report for all samples
* One folder per sample.
  * "sample_name"_consensus.fasta: assembled and polished genome
  * "sample_name"_consensus.gff: genome annotations
  * "sample_name"_consensus.gbk: annotated genome in genbank format
  * "sample_name"_assembly-stats.tab.gff: assembly statistics
  * "sample_name"_kraken2_reads_report.txt: the output from kraken2. The same information can be found in the multi-qc report.
  * Addtional output for debugging can be found in:
    * trimmed
    * sampled
    * prokka
    * mapped_reads
    * consensus



