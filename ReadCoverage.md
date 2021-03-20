## Create plots of read covrage 
````bash
samtools depth -aa -d 1000000 3_1_N.aln-pe.sorted.bam | gzip > 3_1_N.coverage.txt.gz

