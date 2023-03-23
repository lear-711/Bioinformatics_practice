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
`qiime dada2 denoise-single   --i-demultiplexed-seqs sequences.qza   --p-trim-left 35 --p-trunc-len 40 --o-representative-sequences rep-seqs.qza --o-table table.qza --o-denoising-stats stats.qza`

Checking how many reads are passed the filter and were clustered: \
`qiime metadata tabulate   --m-input-file stats.qza   --o-visualization stats.qzv`
