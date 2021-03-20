## Create plots of read covrage using samtools and tidyverse package
````bash
samtools depth -aa -d 1000000 3_1_N.aln-pe.sorted.bam | gzip > 3_1_N.coverage.txt.gz

````R
# Open R consol
install.package("tidyverse")
library(tidyverse)
# Load data
cov <- as.tbl(read.table(file = "coverage.txt.gz")) %>% 
	rename("Position" = V2) %>% rename("Coverage" = V3)
# Plot
cov %>% select(Position, Coverage) %>% 
	ggplot(aes(Position, Coverage)) + 
	geom_line()
