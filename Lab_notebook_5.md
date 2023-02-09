# H+, or how to build a perfect human

### Dataset

1. Download dataset: 23andMe results released under Creative Commons Public Domain License: \
`wget https://raw.githubusercontent.com/msporny/dna/master/ManuSporny-genome.txt`

2. Download plink 1.9 \
[https://www.cog-genomics.org/plink/] \
path to plink: `/Users/macair/Desktop/plink_mac_20230116`

### File conversion

3. Remove all SNPs corresponding to deletions and insertions to make the file compatible with annotation tools: \
`../../../plink_mac_20230116/plink --23file ManuSporny-genome.txt --recode vcf --out snps_clean --output-chr MT --snps-only just-acgt`

### Origins, haplogroups

[Result](https://github.com/lear-711/Bioinformatics_practice/blob/ba39eda8b34ad9c0902146580bbef08fafa7fb51/snps_clean%E2%80%94mtDNA%20Haplogroup%20Analysis%20Report.pdf) for mtDNA (shows all SNPs that distinguish the haplogroup)

### Annotation - sex and eye colour

**SNPs responsible for eye colour and skin tone**: \
*Eye color:* \
 rs12203592: Chromosome - 6, Position - 396321, Gen - IRF4 \
 rs12896399: Chromosome - 14, Position - 92307319, Gene - LOC105370627 \
 rs12913832: Chromosome - 15, Position - 28120472, Gene- HERC2 \
 rs16891982: Chromosome - 5, Position - 33951588, Gene - SLC45A2, G - 7x more likely to have black hair / Light skin; Possibly an increased risk of melanoma\
 rs6119471: Chromosome - 20 \
 
 *Skin coloration:* \
 rs12913832: Chromosome - 15, Position - 28120472, Gene- HERC2 \
 rs16891982: Chromosome - 5, Position - 33951588, Gene - SLC45A2 \
 rs6119471: Chromosome - 20 \
 rs1426654
 rs885479
 rs1545397
 
 ### Annotation of all SNPs, selection of clinically relevant ones
 
[Result](http://grch37.ensembl.org/Homo_sapiens/Tools/VEP/Results?tl=R0Bm79SMKpUCtY9F-8930280) of Variant Effect Predictor

 Look at CLIN_SIG: \
 `awk '($32!="-") ' <vep_output.txt> | grep risk_factor | cut -f 1-3 | sort | uniq`
 
 
 
 
