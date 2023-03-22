# Dead Man’s Teeth. Introduction to metagenomics analysis.

### 1. QIIME2 installation
```
wget https://data.qiime2.org/distro/core/qiime2-2023.2-py38-osx-conda.yml
CONDA_SUBDIR=osx-64 conda env create -n qiime2-2023.2 --file qiime2-2023.2-py38-osx-conda.yml
conda activate qiime2-2023.2
conda config --env --set subdir osx-64
```

### 2. Importing data
`qiime tools import   --type SRR957753.fastq   --input-path manifest.tsv   --output-path sequences.qza   --input-format SingleEndFastqManifestPhred33V2`

Checking correctness of any QIIME artifact with qiime validate: \
`qiime tools validate sequences.qza`
