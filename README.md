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
1. Selecting the suitable protein sequences required for the phylogenetic analysis of the Protein family Cofilin.
   For this project, 10 sequenxes has been manually selected from the Protein Data Bank (https://www.rcsb.org/)
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

   * PHASE-1
     
   - First, all the 10 sequences downloaded in 10 separate fasta files were merged into one fasta file (myFamilyCofilin(curated).fasta)
   -  I have taken each of my non sequences to check for sequence quality using Expasy ProtParam. None of my sequences had any B,Z OR X invalid sequences and the sequences lengths were also maintained well betweeen 160aa- 170aa. Hence no sequences were removed in this step.
   -  Paralogous vertebrates isoforms were identified (CFL-1 and ADF) using UniprotKB. Both the paralogues were kept in the gene sequence for diverse phylogeny analysis.
   -  Next, I ran a redundancy analysis of all my 10 sequences. There I found that only 4 of my sequences (1AK6, 1TVJ, 4BEX and 1CFY) were the only non-redundant sequences. Hence I needed to add more sequences for my phylogenetic analysis.
    

* PHASE-2

  - I downloaded 10 more sequences from Protein Data Bank. They are as follows:
    * 9Y52_3|Chains G[auth a], H[auth b], I[auth c], J[auth d], K[auth e]|Cofilin-2|Homo sapiens (9606)
    * 7U8K_2|Chains K, L, M, N, O, P, Q, R|Cofilin-2|Homo sapiens (9606)
    * 5YU8_2|Chains F[auth H], G[auth I], H[auth J]|Cofilin-2|Gallus gallus (9031)
    * 7Q8S_2|Chains B, D[auth G], F[auth H], H[auth I], J|ADF/Cofilin|Leishmania major (5664)
    * 9Q7M_2|Chains F[auth a], G[auth b], H[auth c], I[auth d]|Cofilin-2|Homo sapiens (9606)
    * 9Y9J_3|Chains H[auth a], I[auth b], J[auth c], K[auth d], L[auth e]|Cofilin-2|Homo sapiens (9606)
    * 1SQC_1|Chain A|SQUALENE-HOPENE CYCLASE|Alicyclobacillus acidocaldarius (405212)
    * AF_AFA4I4A3F1_1|Chain A|ADF/Cofilin|Leishmania infantum (5671)
    * 4KED_1|Chains A, B|Cofilin|Saccharomyces cerevisiae (559292)
    * 7M0G_1|Chain A|Cofilin-2|Homo sapiens (9606)

- After combing all my 14 sequences in one single fasta file (4 selected from PHASE-1 and 10 from now), I again run them for decreasing redundancy using Expasy.
- This time only 42% of the original set were non redundant and only 6 out of the 14 sequences were selected for furthur analysis (the six members are: 1AK6, 4BEX, 1TVJ, 1CFY, 1SQC_1, AF_AFA4I4A3F1_1)

* PHASE-3
  
  -I downloaded further three different sequences for maximum non redundancy. They are as follows:
  * 2MOT_1|Chain A|Actin depolymerizing factor ADF|Toxoplasma gondii (5811)
  * 1F7S_1|Chain A|ACTIN DEPOLYMERIZING FACTOR (ADF)|Arabidopsis thaliana (3702)
  * 4LIZ_1|Chain A|Actin-binding protein, cofilin/tropomyosin family protein, putative|Entamoeba histolytica (885318)

- After combing all my 10 sequences into one fasta file, I examined their redundancy using Expasy (Parameter used: 90% max similarity). Finally all the sequences were non-redundant and the following protein members were chosen for further analysis:
  
  1AK6 (ADF Pig Destrin), 1CFY (Yeast Cofilin), 1SQC_1(ADF/Cofilin family Presumed Cofilin-related), 4BEX(Human Cofilin-1	ADF-H domain (Cofilin-like family) ), 4LIZ_1	(EhCoactosin (Entamoeba histolytica)	ADF-H domain (Cofilin/tropomyosin family)), 7Q8S_2	(Leishmania major cofilin	ADF-H domain (ADF/Cofilin family) ),
  1F7S_1	(Arabidopsis thaliana ADF1	ADF-H domain (Severin superfamily)), 2MOT_1	(Toxoplasma gondii ADF	ADF-H domain), 9FP8_1	((Mosquito ADF - AgADF1)	ADF-H domain (presumed from family annotation)), 1TVJ	(Chick Cofilin	ADF-H domain (Cofilin/ADF family))

- Further tools Scan Prosite (Expasy) and Prot Param were used to evaluate Sequence lengths and unnecessary sequence elements. Finally the Phase 3 protein sequences were curated into a single fasta document (attached).
  

3. Next step involved the Multiple Sequence Alignment and Trimming of MSA.
  
      




