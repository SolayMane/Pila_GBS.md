###make blast databse with the referecen cp genome
````bash
makeblastdb -in Q_lobata_v30_hap.fasta -dbtype nucl -out blastdb/Q_lobata_CP
````
### run a blastn search using the assembly as query
````bash
blastn -query 8_17_Q_durata_S66_L001.fa -db blastdb/Q_lobata_CP -num_descriptions 1 -num_alignments 1 -evalue 0.001 -num_threads 36 > 8_17_Q_durata_vs_Q_lobata.blastn
````
### parsing the blastn output

````bash
cat 8_17_Q_durata_vs_Q_lobata.blastn | awk '/^Query=/ {printf $2" "} /Length/ {printf $0" "}\
/^> chr/ {printf "hit "$1" "$2" "$3" "} /^ Identities/ {print $3,$4} /No hits/ {print "no_hits -- -- -- -- -- --"}' | grep -v 'no_hits' > 8_17_Q_durata_vs_Q_lobata.blastn.filtred
````

