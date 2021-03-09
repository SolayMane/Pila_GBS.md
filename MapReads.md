# We will map the reads on the Pila reference using bwa mem
```bash
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


" now w'll use these names to input files within bwa meme code
ls data/ | sed 's/.[12].fq.gz//g' | sort | uniq | while read sample; do bwa mem -t Pila.1_0.fa \ 
data/{sample}.1.fq.gz data/{sample}.2.fq.gz > out/{sample}.aln-pe.sam ; done
