### make blast databse with the reference cp genome
````bash
makeblastdb -in Q_lobata_v30_hap.fasta -dbtype nucl -out blastdb/Q_lobata_CP
````
### run a blastn search using the assembly as query
````bash
blastn -query 8_17_Q_durata_S66_L001.fa -db blastdb/Q_lobata_CP -max_target_seqs 1 -evalue 0.001 -num_threads 36 -outfmt '6 delim=@ qaccver saccver pident length qlen slen evalue qcovs '> 8_17_Q_durata_vs_Q_lobata.blastn.tab
````
