# Differential RNA expression analysis

### 1. Downloading files for analysis:
`wget http://ftp.sra.ebi.ac.uk/vol1/fastq/SRR941/SRR941816/SRR941816.fastq.gz` \
`wget http://ftp.sra.ebi.ac.uk/vol1/fastq/SRR941/SRR941817/SRR941817.fastq.gz` \
`wget http://ftp.sra.ebi.ac.uk/vol1/fastq/SRR941/SRR941818/SRR941818.fastq.gz` \
`wget http://ftp.sra.ebi.ac.uk/vol1/fastq/SRR941/SRR941819/SRR941819.fastq.gz`

**Unzipping files**: \
`gzcat GCF_000146045.2_R64_genomic.fna.gz > GCF_000146045.2_R64_genomic.fna` \
`gzcat GCF_000146045.2_R64_genomic.gff.gz > GCF_000146045.2_R64_genomic.gff` \
`gzcat SRR941816.fastq.gz > SRR941816.fastq` \
`gzcat SRR941817.fastq.gz > SRR941817.fastq` \
`gzcat SRR941818.fastq.gz > SRR941818.fastq` \
`gzcat SRR941819.fastq.gz > SRR941819.fastq`

### 2. HISAT2

#### 2.1 Installing:
`conda install hisat2`

#### 2.2 Aligning:
Building genome index: \
`hisat2-build GCF_000146045.2_R64_genomic.fna GCF_000146045.2_R64_genomic_ind`

Run hisat2 in single-end mode: \
`hisat2 -p 2 -x GCF_000146045.2_R64_genomic_ind -U SRR941816.fastq | samtools sort > SRR941816_out.bam` \
`hisat2 -p 2 -x GCF_000146045.2_R64_genomic_ind -U SRR941817.fastq | samtools sort > SRR941817_out.bam` \
`hisat2 -p 2 -x GCF_000146045.2_R64_genomic_ind -U SRR941818.fastq | samtools sort > SRR941818_out.bam` \
`hisat2 -p 2 -x GCF_000146045.2_R64_genomic_ind -U SRR941819.fastq | samtools sort > SRR941819_out.bam`

#### 2.3. Quantifying with featureCounts

Install gffread and subread: \
`conda install -c bioconda gffread` \
`conda install -c bioconda subread`

Convert the GFF file to GTF format: \
`gffread GCF_000146045.2_R64_genomic.gff -T -o GCF_000146045.2_R64_genomic.gtf`

Run the feature counts program: \
`featureCounts -g gene_id -a GCF_000146045.2_R64_genomic.gtf -o GCF_000146045.2_R64_genomic_feature_counts  SRR941816_out.bam SRR941817_out.bam SRR941818_out.bam SRR941819_out.bam` 

Simplify the counts: \
`cat GCF_000146045.2_R64_genomic_feature_counts | cut -f 1,7-10 > simple_counts.txt`

#### 2.4 Find differentially expressed genes with Deseq2

**Uploading the files**: \
[deseq2.r](https://github.com/lear-711/Bioinformatics_practice/blob/bef7fd2acf7730e55d7d34f7e03ad031698ac6b6/Project_Differential_RNA_expression_analysis/deseq2.r) \
[draw-heatmap.r](https://github.com/lear-711/Bioinformatics_practice/blob/bef7fd2acf7730e55d7d34f7e03ad031698ac6b6/Project_Differential_RNA_expression_analysis/draw-heatmap.r)

**Calculate metrics**: \
Installing deseq2: \
`conda install -c bioconda bioconductor-deseq2`

`cat simple_counts.txt | R -f deseq2.r ` \
[result.txt](https://github.com/lear-711/Bioinformatics_practice/blob/bef7fd2acf7730e55d7d34f7e03ad031698ac6b6/Project_Differential_RNA_expression_analysis/result.txt) contains calculated metrics for our genes \
[norm-matrix-deseq2.txt](https://github.com/lear-711/Bioinformatics_practice/blob/bef7fd2acf7730e55d7d34f7e03ad031698ac6b6/Project_Differential_RNA_expression_analysis/norm-matrix-deseq2.txt) contains normalised counts that we will use in visualisation

**Draw heatmap**: \
`cat norm-matrix-deseq2.txt | R -f draw-heatmap.r`

Result: [output.pdf](https://github.com/lear-711/Bioinformatics_practice/blob/bef7fd2acf7730e55d7d34f7e03ad031698ac6b6/Project_Differential_RNA_expression_analysis/output.pdf)

### 3. Result Interpretation

3.1 Taking the first 50 genes from file *result.txt* and keeping only the first column (gene names): \
`head -n 50 result.txt | cut -f 1 | cut -d "-" -f 2 > genes.txt` \
[genes.txt](https://github.com/lear-711/Bioinformatics_practice/blob/bef7fd2acf7730e55d7d34f7e03ad031698ac6b6/Project_Differential_RNA_expression_analysis/genes.txt)

3.2 Using [Generic gene ontology (GO) term mapper](https://go.princeton.edu/cgi-bin/GOTermMapper) for analysis

[Result of Gene ontology terms analysis](https://github.com/lear-711/Bioinformatics_practice/blob/bef7fd2acf7730e55d7d34f7e03ad031698ac6b6/Project_Differential_RNA_expression_analysis/18837_slimTerms.pdf) \
([HTML version](https://go.princeton.edu/tmp//18837_slimTerms.html))

