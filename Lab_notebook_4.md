# Tardigrades: from genestealers to space marines

1. Download assembled genome of Ramazzottius varieornatus, one of the most stress-tolerant species of Tardigrades: 
`wget http://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/001/949/185/GCA_001949185.1_Rvar_4.0/GCA_001949185.1_Rvar_4.0_genomic.fna.gz`
2. Download [AUGUSTUS results](https://drive.google.com/file/d/1wBxf6cDgu22NbjAOgTe-8b3Zx60hNKY0/view?usp=drive_web)
3. Download [list of peptides that were associated with the DNA](https://disk.yandex.ru/d/xJqQMGX77Xueqg)
4. Extract protein sequences (fasta) from the prediction output (GFF): 
`./getAnnoFasta.pl augustus.whole.gff`
5. Number of obtained proteins (augustus.whole.aa): `grep ">" augustus.whole.aa | wc -l` = 16435
6. `conda install -c bioconda blast`
7. Create a database: `makeblastdb -in augustus.whole.aa -dbtype prot  -out database_proteins`
8. Performing the search: `blastp -db database_proteins -query peptides.fa -outfmt 6  -out blasted_proteins.fa`
9. Extract proteins of interest: \
`cat blasted_proteins.fa | awk '{print $2}' > names.txt` \
`xargs samtools faidx augustus.whole.aa < names.txt > interest.fa` \
`cat ./interest.fa | grep '>' | wc -l` = 118 proteins of interest

Prediction where these proteins are found in the cell based on their sequences:
10. [Results of WoLF PSORT](https://wolfpsort.hgc.jp/results/aKC99f49e81a7e04690c90d3c2176bf8494.html) \
11. [Results of TargetP 1.1 Server](https://services.healthtech.dtu.dk/service.php?TargetP-2.0) \
12. Based on the results obtained, we filter only those proteins that are found in the nucleus and take only unique ones: \
gg10513.t1 \
gg10514.t1 \
gg11513.t1 \
gg11806.t1 \
gg11960.t1 \ 
gg14472.t1 \
gg16318.t1 \
gg16368.t1 \
gg2203.t1 \
gg3428.t1 \
gg5443.t1 \
gg5927.t1 \
gg7861.t1 \
gg8100.t1 \
gg8312.t1
