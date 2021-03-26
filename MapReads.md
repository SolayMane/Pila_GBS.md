# We will map the reads on the Pila reference using bwa mem
```bash
# download the reference sequence of Pila :
wget https://treegenesdb.org/FTP/Genomes/Pila/v1.0/genome/Pila.1_0.fa.gz
--2021-03-08 15:24:09--  https://treegenesdb.org/FTP/Genomes/Pila/v1.0/genome/Pila.1_0.fa.gz
Resolving treegenesdb.org (treegenesdb.org)... 155.37.254.148
Connecting to treegenesdb.org (treegenesdb.org)|155.37.254.148|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 9404061344 (8.8G) [application/octet-stream]
Saving to: ‘Pila.1_0.fa.gz’

Pila.1_0.fa.gz                  100%[====================================================>]   8.76G  7.78MB/s    in 24m 47s

2021-03-08 15:48:58 (6.03 MB/s) - ‘Pila.1_0.fa.gz’ saved [9404061344/9404061344]

#gunzip the file
gunzip Pila.1_0.fa.gz

# we will index the reference sequence
bwa index Pila.1_0.fa
samtools faidx Pila.1_0.fa

#we will merge the all the R1 file from the samples
cd /data
mkdir merged
for file in 153 157 3 9; do cat ${file}*.1.fq.gz > merged/${file}.1.fq.gz ; done
for file in 153 157 3 9; do cat ${file}*.2.fq.gz > merged/${file}.2.fq.gz ; done

# this line is to retrieve only the names of samples
ls data/merged | sed 's/.[12].fq.gz//g' | sort | uniq
153
157
3
9


#now we will use those names to input files within bwa meme code
ls data/merged | sed 's/.[12].fq.gz//g' | sort | uniq | while read sample
do bwa mem -t 36 ref/Pila.1_0.fa data/merged/${sample}.1.fq.gz data/merged/${sample}.2.fq.gz > out/${sample}.aln-pe.sam
samtools view -bS out/${sample}.aln-pe.sam -o out/${sample}.aln-pe.bam
samtools sort out/${sample}.aln-pe.bam -o out/${sample}.aln-pe.sorted
samtools index out/${sample}.aln-pe.sorted.bam

done

# now we will create a folder in wich we will call cnv from the bam files
mkdir cnv
cp out/*.aln-pe.sorted.bam cnv/
awk -F"\t" '{ print $1,$2}' Pila.1_0.fa.fai > cnv/pila-size_chr.txt
