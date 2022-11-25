# Project 3. E. coli outbreak investigation, De novo assembly and annotation of bacterial genome.

Создаем рабочую директорию **Project_3**.

## Подготовка данных
1. Создаем директорию для сырых данных **Raw_Data** в **Project_3** и переходим в нее.
2. Скачиваем данные секвенирования образца **TY2482**: \
с размером вставки 470 bp - \
прямые риды `wget https://d28rh4a8wq0iu5.cloudfront.net/bioinfo/SRR292678sub_S1_L001_R1_001.fastq.gz`, \
обратные риды `wget https://d28rh4a8wq0iu5.cloudfront.net/bioinfo/SRR292678sub_S1_L001_R2_001.fastq.gz`, \
с размером вставки 2 kb - \
прямые риды `wget https://d28rh4a8wq0iu5.cloudfront.net/bioinfo/SRR292862_S2_L001_R1_001.fastq.gz`, \
обратные риды `wget https://d28rh4a8wq0iu5.cloudfront.net/bioinfo/SRR292862_S2_L001_R2_001.fastq.gz`, \
с размером вставки 6 kb - \
прямые риды `wget https://d28rh4a8wq0iu5.cloudfront.net/bioinfo/SRR292770_S1_L001_R1_001.fastq.gz`, \
обратные риды `wget https://d28rh4a8wq0iu5.cloudfront.net/bioinfo/SRR292770_S1_L001_R2_001.fastq.gz`. \

3. Разархивируем скачанные файлы: `gzcat SRR292678sub_S1_L001_R1_001.fastq.gz > SRR292678sub_S1_L001_R1_001.fastq`,
`gzcat SRR292678sub_S1_L001_R2_001.fastq.gz > SRR292678sub_S1_L001_R2_001.fastq`, `gzcat SRR292862_S2_L001_R1_001.fastq.gz > SRR292862_S2_L001_R1_001.fastq`,
`gzcat SRR292862_S2_L001_R2_001.fastq.gz > SRR292862_S2_L001_R2_001.fastq`, `gzcat SRR292770_S1_L001_R1_001.fastq.gz > SRR292770_S1_L001_R1_001.fastq`,
`gzcat SRR292770_S1_L001_R2_001.fastq.gz > SRR292770_S1_L001_R2_001.fastq`

4. Запускаем программу FastQC на всех 6 файлах для оценки количества ридов: `fastqc -o . ./SRR292678sub_S1_L001_R1_001.fastq ./SRR292678sub_S1_L001_R2_001.fastq ./SRR292770_S1_L001_R1_001.fastq ./SRR292862_S2_L001_R1_001.fastq ./SRR292862_S2_L001_R2_001.fastq ./SRR292770_S1_L001_R2_001.fastq`
5. Получилось следующее количество ридов: \
SRR292678sub_S1_L001_R1_001.fastq - 5499346\
SRR292678sub_S1_L001_R2_001.fastq - 5499346\
SRR292862_S2_L001_R1_001.fastq - 5102041\
SRR292862_S2_L001_R2_001.fastq - 5102041\
SRR292770_S1_L001_R1_001.fastq - 5102041\
SRR292770_S1_L001_R2_001.fastq - 5102041

6. Запускаем программу jellyfish для подсчета частоты всех возможных k-меров заданной длины (31) в наших данных: 
`jellyfish count -m 31 -s 50000000 -t 4 -o 78sub.jf  SRR292678sub_S1_L001_R1_001.fastq SRR292678sub_S1_L001_R2_001.fastq`, 
`jellyfish count -m 31 -s 50000000 -t 4 -o 70_S1.jf  SRR292770_S1_L001_R1_001.fastq SRR292770_S1_L001_R2_001.fastq`,
`jellyfish count -m 31 -s 50000000 -t 4 -o 62_S2.jf  SRR292862_S2_L001_R1_001.fastq SRR292862_S2_L001_R2_001.fastq` \
Делаем файлы с гистограммой : `jellyfish histo 78sub.jf`, `jellyfish histo -o 78sub.histo 78sub.jf`, \
`jellyfish histo 70_S1.jf`, `jellyfish histo -o 70_S1.histo 70_S1.jf`, \
`jellyfish histo 62_S2.jf`, `jellyfish histo -o 62_S2.histo 62_S2.jf`.

7. Графики распределения k-меров: \
[K-mer distribution in sample SRR292862](https://github.com/lear-711/Bioinformatics_practice/blob/c1f49ff22a0a61943484315b93f5f6230235b4ad/Rplot_62.jpeg) \
[K-mer distribution in sample SRR292770](https://github.com/lear-711/Bioinformatics_practice/blob/b54005e13d8272b4a2d073b032e324971486ef09/Rplot_70.jpeg) \
[K-mer distribution in sample SRR292678sub](https://github.com/lear-711/Bioinformatics_practice/blob/b54005e13d8272b4a2d073b032e324971486ef09/Rplot_78sub.jpeg)

8. Делаем символическую ссылку на spadex: `ln -s /Users/macair/Desktop/Bioinformatics_Institute/Bioinf_practice/Project_3/SPAdes-3.15.4-Darwin/bin/spades.py spades.py`

9. Делаем символическую ссылку на quast: `ln -s /Users/macair/Desktop/Bioinformatics_Institute/Bioinf_practice/Project_3/quast-5.2.0/quast.py quast`

10. Запускаем программу spadex для образца SRR292678: 
`./spades.py --pe1-1 SRR292678sub_S1_L001_R1_001.fastq.gz --pe1-2 SRR292678sub_S1_L001_R2_001.fastq.gz -o spades78_result_re`

11. Запускаем программу quast для образца SRR292678:
``


9. Запускаем программу spadex для 3 образцов: 
`./spades.py --pe1-1 SRR292678sub_S1_L001_R1_001.fastq.gz --pe1-2 SRR292678sub_S1_L001_R2_001.fastq.gz --mp1-1 SRR292770_S1_L001_R1_001.fastq.gz --mp1-2 SRR292770_S1_L001_R2_001.fastq.gz --mp2-1 SRR292862_S2_L001_R1_001.fastq.gz --mp2-2 SRR292862_S2_L001_R2_001.fastq.gz -o spades_result_mate`

