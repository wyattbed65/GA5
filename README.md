

# GA5 answers - Wyatt Bednar

------------------------------------------------------------------------

## Part A: Your project dir, Git repo, and protocol

### 1: Fill in the top of the protocol



### 2: Markdown formatting



### 3: Create subdirs, and copy the scripts

```bash
mkdir -p scripts data/ref results
```

### 4: Initialize a Git repository, and commit your first files



### 5: Download the reference transcriptome



Code to count the number of transcripts:

```bash
zgrep ">" data/ref/GCF_016801865.2_TS_CPP_V2_rna.fna.gz | wc -l
```

The output of the command was:

```
28624
```

*Answer*: The number of transcripts in the file:

28624

## Part B: Read trimming

### 6: Run TrimGalore



*Answer*: Whether all batch jobs succeeded, and how you checked that
(and any problems you encountered, and how you solved them):

I encountered a problem in my original code, as I didn't set the correct input directory and had to change it. This is reflected in the Git repository. I also originially messed up the loop, as I put the same line fastq_r1 in "$fastq_dir"/*_R1.fastq.gz to get the second command. It didn't work, and I looked at the notes to revise my loop and change it to the proper format. 

To check for errors, I used the command tail slurm-trimgalore*.out, which printed all of my slurm files and all of them were labeled as successful. 

## Part C: Transcript quantification with Salmon

### 7: Create the Salmon index

Code to check the size of the index dir:

```bash
du -sh results/salmon_index
```

The output of the command was:

```
1.1G    results/salmon_index
```

*Answer*: How long it took to create the index, and how you found out: It took 24 seconds. On the slurm file, it says I submitted it at 9:25:43. When I ran tail tail slurm-salmon_index-57030300.out, it says it was completed at 9:26:07, which is 24 seconds.

*Answer*: The size of the index dir: 1.1 G

### 8: Run Salmon for all samples



### 9: The transcripts with the most reads

*Answer*: What the `NumReads` column contains: It is an estimation for the number of reads matching to each transcript that was quantified.

```bash
sort -k5,5nr results/salmon_quant/S63/quant.sf | head -n 3
```

The output of the command was:

```
XM_039591534.2  6566    6315.086        14518.656821    24004.889
XM_039573735.1  6445    6194.086        5342.073324     8663.254
XM_039573733.1  6591    6340.086        2876.480244     4774.749
```

### 10: The gene of the top transcript

```bash
grep "XM_039591534.2" data/ref/GCF_016801865.2_TS_CPP_V2_rna.fna 
```

The output of the command was:

```
XM_039591534.2 PREDICTED: Culex pipiens pallens vitellogenin-A1-like (LOC120426760), mRNA
```

*Answer*: The gene that the top transcript belongs to: Culex pipiens pallens vitellogenin-A1-like (LOC120426760)

### 11: Mapping rates

```bash
grep "mapping rate" results/salmon_quant/*S*/logs/salmon_quant.log
```

The output of the command was:

```
results/salmon_quant/S63/logs/salmon_quant.log:mapping rate: 82.4560%
results/salmon_quant/S64/logs/salmon_quant.log:mapping rate: 81.9152%
results/salmon_quant/S65/logs/salmon_quant.log:mapping rate: 79.1055%
results/salmon_quant/S66/logs/salmon_quant.log:mapping rate: 81.9350%
results/salmon_quant/S67/logs/salmon_quant.log:mapping rate: 83.0459%
results/salmon_quant/S68/logs/salmon_quant.log:mapping rate: 78.8070%
results/salmon_quant/S69/logs/salmon_quant.log:mapping rate: 82.1824%
results/salmon_quant/S70/logs/salmon_quant.log:mapping rate: 82.3060%
results/salmon_quant/S71/logs/salmon_quant.log:mapping rate: 79.9047%
results/salmon_quant/S74/logs/salmon_quant.log:mapping rate: 78.2753%
results/salmon_quant/S75/logs/salmon_quant.log:mapping rate: 80.3915%
results/salmon_quant/S76/logs/salmon_quant.log:mapping rate: 82.3396%
results/salmon_quant/S77/logs/salmon_quant.log:mapping rate: 82.3281%
results/salmon_quant/S78/logs/salmon_quant.log:mapping rate: 84.2490%
results/salmon_quant/S79/logs/salmon_quant.log:mapping rate: 80.4061%
results/salmon_quant/S80/logs/salmon_quant.log:mapping rate: 82.1721%
results/salmon_quant/S81/logs/salmon_quant.log:mapping rate: 83.6795%
results/salmon_quant/S82/logs/salmon_quant.log:mapping rate: 78.7291%
results/salmon_quant/S83/logs/salmon_quant.log:mapping rate: 76.1504%
results/salmon_quant/S84/logs/salmon_quant.log:mapping rate: 83.2207%
results/salmon_quant/S85/logs/salmon_quant.log:mapping rate: 76.9540%
results/salmon_quant/S86/logs/salmon_quant.log:mapping rate: 83.0532%
```

*Answer*: The sample with the lowest mapping rate:

results/salmon_quant/S82/logs/salmon_quant.log:mapping rate: 78.7291%
S82

## Part D: Summarizing all steps with MultiQC

### 12: Update your MultiQC script

<!-- No need to paste the script: we'll look at scripts/multiqc.sh and its Git history directly. -->

### 13: Run MultiQC

*Answer*: For each program, the number of reports that MultiQC found,
and why it found this number:

salmon | Found 22 meta reports
Salmon is run on each sample, not each read, meaning there are half the reports.
salmon | Found 22 fragment length distributions
salmon | Found 22 library format counts reports
trim_galore | Found 44 reports
Trim Galore looks at both reads (R1 and R2) for each sample. This is why there are double the reports.
fastqc | Found 44 reports
FastQC contains the files for both reads (R1 and R2) for each sample.


### 14: The General Statistics table

*Answer*: The sample with the lowest Salmon mapping rate,
and whether this matches your answer to question 11:

### 15: Adapter lengths

*Answer*: The most common adapter lengths, whether this surprised you,
and why you think these lengths are the most common:

### 16: Ask Claude about the MultiQC report

*Answer*: What you asked, and what you learned from Claude's answer (in your own words):

------------------------------------------------------------------------

## Part E: Improving the workflow with genAI

### 17: Claude's review of your protocol and scripts

*Answer*: At least three of Claude's suggestions,
and whether you agree or disagree with each one, and why:

### 18: Address a suggestion

*Answer*: Which suggestion(s) you addressed, and what you changed:

------------------------------------------------------------------------

## Part F: Publish your repo on GitHub

### 19: Create a GitHub repository and push

<!-- No answer needed here -- we check your GitHub repo directly. -->

### 20: Get a (test) DOI with Zenodo

*Answer*: Your Zenodo Sandbox DOI:
