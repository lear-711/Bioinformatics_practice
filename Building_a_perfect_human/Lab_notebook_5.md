# H+, or how to build a perfect human

[Ссылка на Google Colab](https://colab.research.google.com/drive/17G5jDCQJLDjDRkJI8C8L4vbVCb4-FzHl?usp=sharing) \
[Ссылка на Overleaf](https://www.overleaf.com/read/fmfdyvzwwmkk)

### Dataset

1. Download dataset: 23andMe results released under Creative Commons Public Domain License: \
`wget https://drive.google.com/open?id=1QJkwJe5Xl_jSVpqdTSNXP7sqlYfI666j`

2. Download plink 1.9 \
[https://www.cog-genomics.org/plink/] \
path to plink: `/Users/macair/Desktop/plink_mac_20230116`

### File conversion

3. Remove all SNPs corresponding to deletions and insertions to make the file compatible with annotation tools: \
`../../../plink_mac_20230116/plink --23file SNP_raw_v4_Full_20170514175358.txt --recode vcf --out snps_clean --output-chr MT --snps-only just-acgt`

### Origins, haplogroups

[Result](https://github.com/lear-711/Bioinformatics_practice/blob/f02863f34e924a8757afc4b7fc8f35a36639c2b8/snps_clean_new%E2%80%94mtDNA%20Haplogroup%20Analysis%20Report.pdf) for mtDNA (shows all SNPs that distinguish the haplogroup)

### Annotation - sex and eye colour

**SNPs responsible for eye colour and skin tone**: \
*Eye color:* 
 - rs12203592: Chromosome - 6, Position - 396321, Gen - IRF4 (**C/T** - Primarily in Europeans; likely presence of freckles, brown hair and high sensitivity of skin to sun exposure)
 - rs12896399: Chromosome - 14, Position - 92773663, Gene - LOC105370627 (**G** - NOT lighter hair color & NOT blue eyes more likely)
 - rs12913832: Chromosome - 15, Position - 28365618, Gene- HERC2 (**A/G** - brown eye color)
 - rs16891982: Chromosome - 5, Position - 33951693, Gene - SLC45A2 (**C/G** - if European, 7x more likely to have black hair)
 - rs6119471: Chromosome - 20, Position - 32785212, Gene - ASIP (**C**)
 
 *Skin coloration:*
 - rs12913832: Chromosome - 15, Position - 28365618, Gene- HERC2 (**A/G**)
 - rs16891982: Chromosome - 5, Position - 33951693, Gene - SLC45A2 (**C/G** - if European, 7x more likely to have black hair)
 - rs6119471: Chromosome - 20, Position - 32785212, Gene - ASIP (**C**)
 - rs1426654: Chromosome - 15, Position - 48426484, Gene- MYEF2, SLC24A5  (**A**: AA - probably light-skinned, European ancestry; AG - mixed European + (African or Asian) ancestry possible)
 - rs885479: Chromosome - 16, Position - 89986154, Gene - MC1R (**G** - darker skin color)
 - rs1545397: Chromosome - 15, Position - 28187772, Gene - OCA2 
 
 ### Annotation of all SNPs, selection of clinically relevant ones
 
[Result](http://grch37.ensembl.org/Homo_sapiens/Tools/VEP/Results?tl=4rEMYttflfp7EHls-8953423) of Variant Effect Predictor

 Look at CLIN_SIG: \
 `awk '($32!="-") ' Annotation_2.txt | grep risk_factor | cut -f 1-3 | sort | uniq`
 
 
 
 
