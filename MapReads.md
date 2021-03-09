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

# this line is to retriev only the names of samples
ls data/ | sed 's/.[12].fq.gz//g' | sort | uniq
3_1_N
3_1_N.rem
3_5_N
3_5_N.rem
3_6_N
3_6_N.rem

#now we will use those names to input files within bwa meme code
ls data/ | sed 's/.[12].fq.gz//g' | sort | uniq | while read sample
do bwa mem -t Pila.1_0.fa data/{sample}.1.fq.gz data/{sample}.2.fq.gz > out/{sample}.aln-pe.sam
done
