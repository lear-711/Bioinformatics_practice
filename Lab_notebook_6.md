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

##### 2.1 Installing:
`conda install hisat2`

##### 2.2 Aligning:
Building genome index: \
`hisat2-build GCF_000146045.2_R64_genomic.fna GCF_000146045.2_R64_genomic_ind`

Run hisat2 in single-end mode: \
`hisat2 -p 2 -x GCF_000146045.2_R64_genomic_ind -U SRR941816.fastq | samtools sort > SRR941816_out.bam` \
`hisat2 -p 2 -x GCF_000146045.2_R64_genomic_ind -U SRR941817.fastq | samtools sort > SRR941817_out.bam` \
`hisat2 -p 2 -x GCF_000146045.2_R64_genomic_ind -U SRR941818.fastq | samtools sort > SRR941818_out.bam` \
`hisat2 -p 2 -x GCF_000146045.2_R64_genomic_ind -U SRR941819.fastq | samtools sort > SRR941819_out.bam`

##### Install gffread and subread:
`conda install -c bioconda gffread`

`conda install -c bioconda subread`
