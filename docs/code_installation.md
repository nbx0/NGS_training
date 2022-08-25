# Code installation

## 1. Open a linux powershell
Open up Powershell and type:
```bash
wsl
```
You are now in a linux environment.

The following command will assure you can access the websites to pull the code:
```bash
echo nameserver 8.8.8.8 |sudo tee /etc/resolv.conf
```
## 2. Install miniconda
THe next commands will install [miniconda](https://docs.conda.io/en/latest/miniconda.html):
```bash
un=$( wslpath "$(wslvar USERPROFILE)" |cut -d '/' -f 5)
 curl  -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
 bash Miniconda3-latest-Linux-x86_64.sh -b -u
alias conda=/home/${un}/miniconda3/bin/conda
conda install -c conda-forge mamba
alias mamba=/home/${un}/miniconda3/bin/mamba
```
## 3. Clone the spike-snake repository
```bash
git clone https://github.com/nbx0/SC2-spike-seq.git
```

## 4. You are ready to process your fastqs
[Click here for instructions](./genome_assembly.md)