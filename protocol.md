# RNA-seq analysis of _Culex pipiens_

- Author: Wyatt Bednar
- Date created: 11/3/2026
- Date last modified: 11/3/2026
- This was run at the Pitzer cluster of the Ohio Supercomputer Center (<https://www.osc.edu>)
- OSC working dir: GA5
- GitHub repository: 

## Prerequisites

- **Input files**:
  - Paired-end RNA-seq reads are in `../garrigos/fastq`

- **Software**:
  - The scripts run programs using Apptainer containers,
    with links contained inside the scripts.
  - If running at OSC, no software installations are needed.
  - If you're not at OSC, you need to have Apptainer and Slurm installed.

- Run all code from the working dir listed above.
  You don't need to create the output dirs, because the scripts will do so.

- Each script writes a Slurm log file (`slurm-<script>-<jobID>.out`) to the working dir.
  After the jobs of a step have finished, check these logs for errors,
  then move them into a `logs/` dir within that step's output dir.

## Set up: Download the reference transcriptome FASTA file from NCBI

```bash
wget -P data/ref/ https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/016/801/865/GCF_016801865.2_TS_CPP_V2/GCF_016801865.2_TS_CPP_V2_rna.fna.gz
```

## Set up: Set file paths

```bash
# Inputs:
fastq_dir=../garrigos/fastq
ref_transcripts=data/ref/GCF_016801865.2_TS_CPP_V2_rna.fna.gz

# Base output dir:
outdir=results
```

## Step 1: Read trimming with TrimGalore

bash
```
for fastq_r1 in "$fastq_dir"/*_R1.fastq.gz; do
    fastq_r2="${fastq_r1/_R1/_R2}"
    sbatch scripts/trimgalore.sh "$fastq_r1" "$fastq_r2" results/trimgalore
done
```

## Step 2: Creating a transcriptome index with Salmon

bash
```
mkdir -p results/salmon_index

sbatch scripts/salmon_index.sh $ref_transcripts results/salmon_index
```

## Step 3: Pseudo-alignment and quantification with Salmon

bash
```
mkdir -p results/salmon_quant

for r1 in results/trimgalore/*_R1.fastq.gz; do
    r2="${r1/_R1/_R2}"
    sbatch scripts/salmon_quant.sh "$r1" "$r2" results/salmon_index results/salmon_quant
done
```

## Step 4: Summarizing the results of all steps with MultiQC

bash
```
mkdir -p results/multiqc

sbatch scripts/multiqc.sh results/ results/multiqc
```

