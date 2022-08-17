# SARS-CoV-2 spike-gene assembly with EDGE EC-19
## Expects demultiplexed reads from Oxford Nanopore Technologies sequencer
#
## 1. Install conda

## 2. Clone spike-snake repository

## 3. Build conda environment

## 4. Build samplesheet

## 5. Run spike-seq snakemake process


## 1. Navigate to the working directory with demultiplexed fastqs
```bash
cd /mnt/c/Users/$(whoami)/Desktop/data
```
## 2. Launch IRMA
```bash
for i in $(ls *fastq.gz |sed "s/_R[12].\+//" | sort |uniq);
    do docker run \
        --rm \
        -v $PWD:/data \
        public.ecr.aws/n3z8t4o2/pipeline/irma:1.0.2 \
        IRMA CoV-spike ${i}* $i > IRMA_${i}.stdout 2> IRMA_${i}.stderr
done
```
## 3. View coverage
```bash
for i in  */figures/*coverage*; do wslview $i; done
```
## 4. Concatenate all barcode consensus sequences together by gene
```bash 
cat */amended_consensus/*${i}.fa > amended_consensus_${IAV[$i]}.fasta
```

## 5. Annotate genes into open reading frames (ORFs) with DAIS-Ribosome
```bash
# The next commands will create the input file
today=$(date "+%Y%m%d")
input=${today}_consensus.fasta
output="${today}_ORF.seq ${today}_ORF.ins ${today}_ORF.del"
cat amended_consensus/*fasta >> $input
curl https://raw.githubusercontent.com/nbx0/NGS_training/master/lib/DAIS-Ribosome_refs/A_H1_H3_refs.fasta >> $input

# The next command will run DAIS-ribosome
docker run \
    --rm \
    -v $PWD:/data \
    public.ecr.aws/n3z8t4o2/dais-ribosome:0.1 ribosome \
    --module INFLUENZA $input $output

# The next command will sort the ORFs into seperate fastas to view in BioEdit.
for i in $(cut --output-delimiter=".+" -f 3,4 20220809.seq |sort |uniq)
    do grep -E "${i}\s" 20220809.seq | sed 's/^/>/' |cut -f 1,4,6| sed 's/\t/_/' |tr '\t' '\n' > $(echo $i |cut -d '+' -f 2)_ORF.fasta && wslview $(echo $i |cut -d '+' -f 2)_ORF.fasta
done
```
## 6. Review sequences in BioEdit and assure there are no stop codons (X) inside reading frames

## 7. Your sequences are ready for
- ### [Public Repository Submission](https://github.com/CDCgov/seqsender)
- ### [Nextclade](https://clades.nextstrain.org/)
 

#

## Oxford Nanopore Technologies