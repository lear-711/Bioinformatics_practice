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
8. Performing the search: `blastp -db database_proteins -query peptides.fa -outfmt 6  -out blasted_proteins`
