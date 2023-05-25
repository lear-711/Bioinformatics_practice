# Results of bioinformatics projects

You can see .pdf files with results of my last bioinformatics projects in this repository. \
Here I will provide abstracts of articles describing the results of the projects so that you can quickly see the essence of each of the works.

### 1. Analysis of influenza mutations affecting antibiotic resistance
This work is aimed at studying the cause of the disease despite the flu vaccination. We analyzed data from targeted deep sequencing of a patient with a new strain of the H3N2 influenza virus and tried to find the mutation that is most likely to cause the disease. 3 controls were also analyzed to correctly interpret sequencing errors and real mutations. Average and standard deviations of noise frequencies were calculated. Based on these values, the obtained SNPs in the test sample were filtered and only SNPs which frequencies differ from the average noise frequency by three standard deviations of the noise frequency were analyzed. It was clarified that mutation 307 C→T (0.94) is the only non-synonymous among all found (Pro → Ser).

### 2. Building a perfect human
Microarray genotyping is becoming more and more widely used in genetic laboratory diagnostics, due to the low cost of research and the convenience in choosing a panel of genes based on a particular case. In this work, we used raw 23andMe data with the information about 700k known SNPs. We tried to determine the sex of the patient, the color of his eyes and skin from the information about SNPs. We also proposed to make changes to some SNPs using the technology CRISPR/Cas9 and explained the mechanism of action and the biological value of the changes.

### 3. Differential RNA expression analysis
Differential expression analysis is a classical method based on the RNA-seq technique, which allows researchers to obtain dynamic results about cell behavior in the ever-changing environment Costa-Silva et al. (2017). It allows us to directly observe changes in expression of particular genes. As these changes proved to be adaptive Slotte et al. (2007), we as researchers and spectators can perform observations and further studies of the functions of genes involved in that adaptation process. In this paper, we implemented a differential expression pipeline to determine Saccharomyces cerevisiae genes that are up- or downregulated during the intricate fermentation process leading to dough rising.

### 4. E.coli outbreak investigation
In 2011, an outbreak of the disease caused by E. Coli occurred in Germany, accompanied by hemolytic uremic syndrome (HUS). It was not clear which bacterial strain caused this disease, so in this work we investigated it: the precursor strain, distinctive features (genes and phenotypic traits). We worked with Illumina reads from the TY2482 sample, which were generated at Beijing Genome Institute. De novo assembly of the genome of the bacterium was carried out, a bacterial strain was found that was as similar as possible to the studied strain E.Coli X. Further, the genomes of these two strains were compared, and it was found that genes of Shiga-toxins (stxA, stxB), which are the cause of the disease, were obtained by bacteriophage-mediated horizontal gene transfer. Also, the new strain was distinguished by an antibiotic resistance to the following groups of antibiotics: beta-lactam, aminoglycoside, tetracycline.

### 5. Microbiome exploring
Sequencing samples from a habitat, metagenomics could describe the composition and functional diversity of communities living there. In this research, dental calculus as intact fossils of oral microbiome is analyzed from medieval skeletons along with their teeth roots. The periodontal damage correlates with the presence of the red complex of specific bacteria. In comparison to this preserved genome of Tannerella forsythia, new transposons and genes associated with antibiotic resistance are found in the modern reference.

### 6. Functional annotation of Tardigrade genome assembly reveals potential radiation-resistance genes
It is well-known that high-dose of radiation could cause severe DNA lesions, including double-strand breaks, and leads to genome instability and less viability. However, microscopic eukaryotes from the Tardigrade group have a surprisingly high level of the radio-resistance and can withstand several thousand Gray units of ionizing radiation when the lethal dose for mammals is less than ten Hashimoto and Kunieda (2017). In this study, we performed the functional analysis of the annotated genome assembly of Ramazzottius varieornatus to find genes that could play important roles in the radio-resistance. We looked for the DNA-biding proteins that are localized in the nucleus and found several potentially essential proteins. One of them was the Damage suppressor protein, which was already described by multiple studies as a key regulator of DNA radiation resistance.

### 7. What causes antibiotic resistance?
One of the biggest problems in medicine nowadays is antibiotic resistance, and so many studies are trying to find the reasons for this harmful effect. This lab work has taken several steps to find out the mutations in E. coli strain K-12 substrain MG1655 and the relationship between these and the antibiotic resistance in this E. coli strain. We were identified 3 genes, which products (proteins) are most likely involved in the development of antibiotic resistance in the studied strain of E.coli: FtsI (Peptidoglycan D,D-transpeptidase FtsI), AcrB (Multidrug efflux pump subunit AcrB), EnvZ (Sensor histidine kinase EnvZ).