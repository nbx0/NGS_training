# Influenza genome assembly
## [Illumina](./influenza.md#illumina) or [Oxford Nanopore](./influenza.md#oxford-nanopore-technologies)

## Illumina
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
        IRMA FLU ${i}* $i > IRMA_${i}.stdout 2> IRMA_${i}.stderr
done
```
## 3. View coverage
```bash
for i in  */figures/*coverage*; do wslview $i; done
```
## 3. Initialize a bash array of FLU segment names : numbers
```bash
declare -A IAV
IAV['PB2']=1
IAV['PB1']=2
IAV['PA']=3
IAV['HA']=4
IAV['NP']=5
IAV['NA']=6
IAV['M']=7
IAV['MP']=7
IAV['NS']=8
IAV[1]='PB2'
IAV[2]='PB1'
IAV[3]='PA'
IAV[4]='HA'
IAV[5]='NP'
IAV[6]='NA'
IAV[7]='M'
IAV[8]='NS'
```

## 4. Concatenate Flu sequences together by gene
```bash
for i in 1 2 3 4 5 6 7 8; 
    do cat */amended_consensus/*${i}.fa > amended_consensus_${IAV[$i]}.fasta
done
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