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

## Сборка генома

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

Средняя длина ридов равна **90**\
Приблизительное количество нуклеотидов во всех ридах равно 5499346 * 2 * 90 = **989882280** \
Приблизительная длина генома считается по формуле: \
T/N\ 
N = (M\*L)/(L-K+1) ((N: глубина покрытия, M: пик k-меров, K: размер k-мера, L: средняя длина рида, T: кол-во оснований)) \
**Размер генома** = 989882280 / (125 * 90 / (90 - 31 + 1)) = **5279372** п.н.

8. Делаем символическую ссылку на spadex: `ln -s /Users/macair/Desktop/Bioinformatics_Institute/Bioinf_practice/Project_3/SPAdes-3.15.4-Darwin/bin/spades.py spades.py`

9. Делаем символическую ссылку на quast: `ln -s /Users/macair/Desktop/Bioinformatics_Institute/Bioinf_practice/Project_3/quast-5.2.0/quast.py quast`

10. Запускаем программу spadex для образца SRR292678: \
`./spades.py --pe1-1 SRR292678sub_S1_L001_R1_001.fastq.gz --pe1-2 SRR292678sub_S1_L001_R2_001.fastq.gz -o spades78_result_re`

11. Смотрим на распределение k-меров после spades у corrected файлов: \
`jellyfish count -m 31 -s 50000000 -t 4 -o 78sub_corr.jf  -C SRR292678sub_S1_L001_R1_001.fastq.00.0_0.cor.fastq SRR292678sub_S1_L001_R2_001.fastq.00.0_0.cor.fastq`, `jellyfish histo 78sub_corr.jf`, `jellyfish histo -o 78sub_corr.histo 78sub_corr.jf` \
[K-mer distribution in sample SRR292678sub corrected](https://github.com/lear-711/Bioinformatics_practice/blob/b905d77ea37a241eb47538ced17e6f575cc0ac63/Project_3/Rplot_78_sub_corr.jpeg)

После коррекции ошибок (у corrected файлов) на графике видно, что количество k-меров, которые встречаются 1-2 раза (скорее всего содержащие ошибки секвенирования), заметно сократилось; пик с основными k-мерами стал более выраженным и сдвинулся со значения 64 на 125 (покрытие увеличилось?)

12. Запускаем программу quast для образца SRR292678: \
`./quast ./spades78_result_re/contigs.fasta ./spades78_result_re/scaffolds.fasta`

13. В результате получаем следующие файлы в report: \
[Quast report](https://github.com/lear-711/Bioinformatics_practice/blob/b905d77ea37a241eb47538ced17e6f575cc0ac63/Project_3/Raw_data/quast_results/results_2022_11_26_15_11_17/report.pdf) \
[Basic stats](https://github.com/lear-711/Bioinformatics_practice/tree/Project_3/Project_3/Raw_data/quast_results/results_2022_11_26_15_11_17/basic_stats)

Количество контигов = 204 \
N50 = 105346


14. Запускаем программу spadex для 3 образцов: \
`./spades.py --pe1-1 SRR292678sub_S1_L001_R1_001.fastq.gz --pe1-2 SRR292678sub_S1_L001_R2_001.fastq.gz --mp1-1 SRR292770_S1_L001_R1_001.fastq.gz --mp1-2 SRR292770_S1_L001_R2_001.fastq.gz --mp2-1 SRR292862_S2_L001_R1_001.fastq.gz --mp2-2 SRR292862_S2_L001_R2_001.fastq.gz -o spades_result_mate`

15. Запускаем программу quast для 3 библиотек: `./quast ./spades_result_mate/contigs.fasta ./spades_result_mate/scaffolds.fasta`

16. В результате получаем следующие файлы в report: \
[Quast report](https://github.com/lear-711/Bioinformatics_practice/blob/9e8bc8d237e8a3807034720dbcd847a86d628458/Project_3/Raw_data/quast_results/results_2022_11_28_12_22_15/report.pdf) \
[Basic stats](https://github.com/lear-711/Bioinformatics_practice/tree/Project_3/Project_3/Raw_data/quast_results/results_2022_11_28_12_22_15/basic_stats)

Количество контигов = 176 \
N50 = 151014

По сравнению с предыдущими показателями, количество контигов уменьшилось, а N50 увеличилось, что говорит о лучшем качестве сборки.

## Аннотация генома

17. Запускаем программу prokka: `prokka ./scaffolds.fasta --centre BI`
18. Запускаем программу barrnap: `barrnap -o rrna.fa < contigs.fasta > rrna.gff`
В результате получили нуклеотидную последовательность 16s РНК.

19. Используя данную последовательность, ищем наиболее близкий штамм *E.Coli* на сайте (http://blast.ncbi.nlm.nih.gov/) с параметрами: “Database” - Reference Genome Database (refseq_genomes), “Organism” - Escherichia coli, "Entrez Query" - 1900/01/01:2011/01/01\[PDAT]. Программа вывела 1 штамм с наибольшим процентом совпадения - Escherichia coli 55989 (NCBI Reference Sequence: NC_011748.1) [https://www.ncbi.nlm.nih.gov/nucleotide/NC_011748.1?report=genbank&log$=nuclalign&blast_rank=1&RID=S752C12U013]. Скачиваем нуклеотидную последовательность 16s РНК найденного штамма в формате .fasta.

20. В программе Mauve скачанную последовательность используем в качестве референса, чтобы понять природу возникновения изучаемого нами нового штамма *E.Coli X*

Гены, ассоциированные с Shiga-токсином, полученные при выравнивании в Mauve:
stxB 3483605-3483874
stxA 34838886-3484845

Вокруг генов stxA и stxB находятся гены, кодирующие фаговые белки. Поэтому можно сделать вывод, что природа возникновения нового штамма - фаги, которые "передали" свой собственный геном (профаг) в бактериальный геном.

## Изучение антибиотикорезистентности

Для того, чтобы понять, с каким антибиотикам резистентен изучаемый штамм *E.Coli*, воспользуемся базой данных [https://cge.food.dtu.dk/services/ResFinder/] \

(Antibiotic resistance E.Coli X)[https://cge.food.dtu.dk//cgi-bin/webface.fcgi?jobid=6383B047000073D0DD294E94]
(Antibiotic resistance E.Coli 55989)[]

Антибиотики, к которым резистентен изучаемый штамм *E.Coli X*:

Антибиотики, к которым резистентен штамм *E.Coli 55989*: 



