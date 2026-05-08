# Phylogeny-Task--Cofilin
Phylogenetic Analysis of Protein Family- Cofilin
# Protein phylogeny project
This repository contains a phylogenetic analysis of the protein family Cofilin.
## Folder structure

- data_raw/       - Original downloaded FASTA files (unchanged)
- data_curated/   - Cleaned/filtered FASTA files
- msa_raw/        - Untrimmed multiple sequence alignments
- msa_trimmed/    - Trimmed multiple sequence alignments
- trees/          - Phylogenetic tree files (.treefile, .nwk, etc.)
- docs/           -Extra documentation, figures, notes

## Workflow Steps
1. First we begin by selecting the suitable protein sequences required for the phylogenetic analysis of the Protein family Cofilin.
   For this project, I have manually selected 10 protein sequences in fasta format from Protein DataBank (https://www.rcsb.org/)
   The protein sequences are as follows:
   - 1AK6 DESTRIN / HUMAN ADF (https://www.rcsb.org/structure/1AK6) [downloaded on 6/5/2026]
   - 1CFY YEAST COFILIN (https://www.rcsb.org/structure/1CFY)  [downloaded on 6/5/2026]
   - 1Q8X HUMAN COFILIN-1  (https://www.rcsb.org/structure/1Q8X)  [downloaded on 6/5/2026]
   - 4BEX Crystal structure of human Cofilin‑1 (https://www.rcsb.org/structure/4BEX)  [downloaded on 6/5/2026]
   - 1Q8G HUMAN COFILIN (https://www.rcsb.org/structure/1Q8G) [downloaded on 6/5/2026]
   - 1TVJ Cofilin from Gallus gallus (https://www.rcsb.org/structure/1TVJ) [downloaded on 6/5/2026]
   - 1QPV YEAST COFILIN (https://www.rcsb.org/structure/1QPV) [downloaded on 6/5/2026]
   - 6VAO Human cofilin 1 decorated actin filament (https://www.rcsb.org/structure/6VAO) [downloaded on 6/5/2026]
   - 9QFD cofilin‑decorated actin filaments (https://www.rcsb.org/structure/9QFD)  [downloaded on 6/5/2026]
   - 1COF Yeast Cofilin from Saccharomyces cerevisiae (https://www.rcsb.org/structure/1COF) [downloaded on 6/5/2026]

2. Next step involves the cleaning and curating of the datasets
   - First, all the 10 sequences downloaded in 10 separate fasta files were merged into one fasta file (myFamilyCofilin(curated).fasta)
   - 


