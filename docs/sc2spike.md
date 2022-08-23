# SARS-CoV-2 spike-gene assembly with EDGE EC-19
## Expects demultiplexed reads from Oxford Nanopore Technologies sequencer
#

# Computer setup
## 1. Install conda
**If you have already installed conda and created the pipeline environment, skip to step 4**

Open up Powershell and type:
```bash
wsl
```
You are now in a linux environment.
```bash
 curl  -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
 bash Miniconda3-latest-Linux-x86_64.sh -b -u
alias conda=/home/$(whoami)/miniconda3/bin/conda
conda install -c conda-forge mamba
alias mamba=/home/$(whoami)/miniconda3/bin/mamba
```
## 2. Clone spike-snake repository
```bash
git clone https://github.com/nbx0/SC2-spike-seq.git
```
## 3. Build conda environment
```bash
mamba env create --file ./spike-snake/envs/snakemake_environment.yml -p spikeseq

```
## 4. Activate the conda environment
```bash
source ~/miniconda3/bin/activate spikeseq
```
## 5. Build samplesheet
A simple comma-separated file needs to be generated with your sample names and sequencing run metadata. Create a file with two columns:
| sample name | barcode |
|--|--|
| sample 1 | barcode01 |
| sample 2 | barcode02 |
| . . . | . . . |
| sample N | barcodeNN |
**It is critical to 0-pad numerals 1-9, ie 01, 02, 03...**
Which when saved as a file named _samplesheet.csv_ the textfile should look like:
```bash
sample name,barcode
sample1,barcode01
sample2,barcode02
```

## 6. Run spike-seq snakemake process
**Make sure that Docker is running. You likely need to open the Docker application from the bottom right Windows search box**
```bash
# The next command creates a configuration file required by snakemake
python scripts/config_create.py <path/to/samplesheet.csv> <run_id> <barcode_kit>
# The next command runs primer trimming and IRMA.

```

## 7. Your sequences are ready for
- ### [Public Repository Submission](https://github.com/CDCgov/seqsender)
- ### [Nextclade](https://clades.nextstrain.org/)
 
