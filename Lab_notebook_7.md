# Dead Man’s Teeth. Introduction to metagenomics analysis.

### Part 1. Amplicon sequencing

#### 1. QIIME2 installation
```
wget https://data.qiime2.org/distro/core/qiime2-2023.2-py38-osx-conda.yml
CONDA_SUBDIR=osx-64 conda env create -n qiime2-2023.2 --file qiime2-2023.2-py38-osx-conda.yml
conda activate qiime2-2023.2
conda config --env --set subdir osx-64
```

#### 2. Importing data
`qiime tools import   --type SRR957753.fastq   --input-path manifest.tsv   --output-path sequences.qza   --input-format SingleEndFastqManifestPhred33V2`

Checking correctness of any QIIME artifact with qiime validate: \
`qiime tools validate sequences.qza`

#### 3. Demultiplexing and QC
Exploring how many sequences were obtained per sample and getting a summary of the distribution of sequence qualities: \
`qiime demux summarize --i-data sequences.qza --o-visualization sequences.qzv`

#### 4. Feature table construction (and more QC)
The metadata table is a barcode that was used to amplify the V5 region of the rRNA. \
Striping it out and filtering chimeric sequences (using the DADA2 pipeline): \
`qiime dada2 denoise-single   --i-demultiplexed-seqs sequences.qza   --p-trim-left 35 --p-trunc-len 140 --o-representative-sequences rep-seqs.qza --o-table table.qza --o-denoising-stats stats.qza`

Checking how many reads are passed the filter and were clustered: \
`qiime metadata tabulate   --m-input-file stats.qza   --o-visualization stats.qzv`

#### 5. FeatureTable and FeatureData summaries
Creating visual summaries of the data - how many sequences are associated with each sample and with each feature, etc.: \
`qiime feature-table summarize   --i-table table.qza   --o-visualization table.qzv   --m-sample-metadata-file sample-metadata.tsv`

Mapping feature IDs to sequences to use these representative sequences in other applications, e.g. BLAST each sequence against the NCBI nt database: \
`qiime feature-table tabulate-seqs  --i-data rep-seqs.qza   --o-visualization rep-seqs.qzv`

#### 6. Taxonomic analysis

[Downloading database](https://disk.yandex.ru/d/QxQWKV8x5ucxvw) \
Comparing the representative sequences with the taxonomy database: \
`qiime feature-classifier classify-sklearn   --i-classifier silva-138-99-nb-classifier.qza   --i-reads rep-seqs.qza   --o-classification taxonomy.qza`

Visualisation: \
`qiime metadata tabulate --m-input-file taxonomy.qza --o-visualization taxonomy.qzv`

View the taxonomic composition of our samples with interactive bar plots. \
Generation those plots:
```
qiime taxa barplot \
  --i-table table.qza \
  --i-taxonomy taxonomy.qza \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization taxa-bar-plots.qzv
```
View visualisation:\
`qiime tools view taxa-bar-plots.qzv`

[Q2_taxa barplot](https://github.com/lear-711/Bioinformatics_practice/blob/70c77fcb6a5361e8c79eefe4f0866b7cdfe4f7a6/Data_project_7/q2_taxa%20:%20barplot.pdf)

The three members of the red complex are:
* Porphyromonas gingivalis
* Tannerella forsythia
* Treponema denticola

On barplot we can see that sample S10-V5-Q-B61-calc contains all 3 members of the red complex.

### Part 2. Shotgun sequencing

[Downloading data](https://www.dropbox.com/s/f5j52tliumt6etm/G12_assembly.fna.gz?dl=0)

#### 1. Shotgun sequence data profiling

Installing MetaPhlAn: \
`conda install -c bioconda metaphlan`

MetaPhlAn will align our sequencing reads to the microbiota database, then tabulate the abundance of each type of microbe that matched: \ 
`metaphlan G12_assembly.fna --input_type fasta > output.txt`

#### 2. Comparison with samples from HMP

Comparing our results with data from the Human Microbiome Project: \
```
for f in ./Human_Microbiome_Project; do
        metaphlan $f --input_type fasta --nproc 4 > ${f%.fasta.gz}_profile.txt
done
```

```
for f in *.fasta; do
        metaphlan $f --input_type fasta --nproc 10 > ${f%.fasta.gz}_profile.txt
done
```

#### 3. Visualization of the metaphlan results as a Sankey diagram

**Visualization of the metaphlan results with a heat map** 

Merging the metaphlan profile files into a single abundance table: \
`merge_metaphlan_tables.py ./Human_Microbiome_Project/*profile.txt > merge_output.txt`

Making a basic heat map: \
`python3.8 metaphlan_hclust_heatmap.py --in merge_output.txt --out merge_output_heatmat.png -s log --top 50` ?????


#### 4. Comparison with ancient Tannerella forsythia genome

Aligning contigs on the downloaded reference: \
`bwa index Tannerella_forsythia_92A2.fasta` \
```
for f in SRR9*; do
    bwa mem Tannerella_forsythia_92A2.fasta $f > ${f}_alignment.sam
done
```
```
for f in *alignment.sam; do
    samtools view -S -b $f > ${f%.alignment.sam}_alignment.bam
done
```
Turning obtained alignment file (BAM) to BED: \
```
for f in *alignment.bam; do
    bedtools bamtobed -i $f > ${f}.bed
done
```
```
for f in *.bed; do
    bedtools intersect -a Tannerella_forsythia_92A2.gff3 -b $f -v > ${f}_intersect.bam
done
```
bedtools intersect -a Tannerella_forsythia_92A2.gff3 -b SRR986782.bed -v













