# Genome assemly of Influenza or SARS-CoV-2 spike-only sequencing
## For use on Oxford Nanopore Technologies demultiplexed fastqs

_If you are not already in a powershell, open it now and activate WSL by:_
```bash
wsl
echo nameserver 8.8.8.8 |sudo tee /etc/resolv.conf
```

## 1. Navigate to the working directory with demultiplexed fastqs
```bash
un=$( wslpath "$(wslvar USERPROFILE)" |cut -d '/' -f 5)
cd /mnt/c/Users/${un}/Desktop/data
ls
# Determine which folder your current run's data is located and move to that directory:
# cd <name_of_run_dir>
```

## 2. Activate the conda environment
```bash
source ~/miniconda3/bin/activate spikeseq
```
## 3. Build samplesheet
A simple comma-separated file needs to be generated with your sample names and sequencing run metadata. Create a file with two columns:
| sample name | barcode |
|--|--|
| sample_1 | barcode01 |
| sample_2 | barcode02 |
| . . . | . . . |
| sample_N | barcodeNN |
**It is critical to 0-pad numerals 1-9, ie 01, 02, 03... as well as to have no spaces in your sample names or barcodes. The header must also be exactly as written below (with a space between "sample name".**
Which when saved as a file named _samplesheet.csv_ the textfile should look like:
```bash
sample name,barcode
sample_1,barcode01
sample_2,barcode02
```

**Make sure that Docker is running. You likely need to open the Docker application from the bottom right Windows search box**

## 4. Create the config file needed to run the analyses:
```bash
python scripts/config_create.py <path/to/samplesheet.csv> <run_id> <barcode_kit>
```

## 5. Run the analyses pipeline:
### For SARS-CoV-2 spike-only:
```bash
snakemake -s ~/projects/spike-snake/workflow/sc2_spike_snakefile -j all --use-conda --restart-times 3
```
### For Influenza:
```bash
snakemake -s ~/projects/spike-snake/workflow/influenza_snakefile -j all --use-conda --restart-times 3
```
## 6. Review your coverage files and amino acid alignements for influenza or the _irma_qc_summary.csv_ file for sars-cov-2.

## 7. Your sequences are ready for
- ### [Public Repository Submission](https://github.com/CDCgov/seqsender)
- ### [Nextclade](https://clades.nextstrain.org/)
 
