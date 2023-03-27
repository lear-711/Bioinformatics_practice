# Dead Man’s Teeth. Introduction to metagenomics analysis.

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

