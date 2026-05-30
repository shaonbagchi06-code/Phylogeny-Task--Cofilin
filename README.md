# PHYLOGENETIC ANALYSIS OF PROTEIN FAMILY- COFILIN

Overview:
This repository documents the complete phylogenetic workflow for the protein family Cofilin. Analysis includes dataset curation, multiple sequence alignment, trimming and tree inference using standard tools. Results reveal that that the Cofilin family is evolutionarily conserved, with most members clustering closely together and only a few showing longer branches. The presence of clearly separated sub-branches indicates that while the core protein function is retained, some lineages have accumulated sequence changes over time

## FOLDER STRUCTURE
  - data_raw/ – Original downloaded FASTA files 
  - data_curated/ – Cleaned and filtered FASTA
  - msa_raw/ – Untrimmed MSA files
  - msa_trimmed/ – Trimmed MSA files
  - trees/ – Tree output files (.treefile, .nwk, etc.)
  - scripts/ – Python Commands
  - docs/ – Any notes, figures, or extra documentation
  - README.md – Main workflow description

## WORKFLOW STEPS

1. Dataset Retreival and Curation:
   - Source: Retreived 30 sequences from Protein Data Bank manually for the Cofilin family and corresponding annotations were checked on SwissProt to ensure reliable protein identity and classification.
   - Tools/Database: PDB and SwissProt were used to obtain curated protein records and verify sequence quality
   - Curation Descisions:
     *  Redundant sequences were removed by SwissPort and only the 20 representative non duplicating sequences were retained [1AK6_A (Cow), 1SQC_1 (Slime mold), 7Q8S_2 (Fission yeast), 9FP8_1 (Malaria mosquito), 2MOT_1 (Arabidopsis), 4LIZ_1 (Rice), 1F7S_1 (Arabidopsis), NP_001076577.1 (Dictyostelium), XP_001350375.1 (Plasmodium), AAI66724.1 (Zebrafish), NP_001020250.1 (Chicken), NP_001192114.1 (Arabidopsis), NP_197780.1 (Arabidopsis), P10949.1 (Tetrahymena), KAI3654753.1 (Thermococcus), O14413 (Human), NP_001102452.2 (Rat), A0A024G0G8 (Leishmania), G0W6H8_O42512 (Chaetomium globosum / Zebrafish), P45596 (C. elegans)]
     *  ProtParam was used to examine basic physicochemical properties and help confirm sequence consistency.
     *  The final dataset was curated to include a non-redundant set of Cofilin homologs suitable for phylogenetic analysis.
   - Rationale Behind choosing all the sequences: The initial 30 sequences were selected according to standard methods for phylogenetic analysis of the ADF/CFL gene family, which emphasize comprehensive taxonomic sampling and inclusion of known functional diversity . Specifically, the selection aimed to: (i) represent major evolutionary lineages, including vertebrates (e.g., human, mouse, zebrafish, chicken), plants (e.g., Arabidopsis, rice), fungi (e.g., yeast), and protists (e.g., Dictyostelium, Plasmodium), to capture deep evolutionary relationships and provide outgroups for rooting the tree ; (ii) include multiple paralogs (e.g., cofilin-1, cofilin-2, destrin) because the ADF/CFL family underwent gene duplications in vertebrate and plant lineages, producing functionally distinct subclasses that are tissue-specifically expressed and have diverged biochemical activities ; and (iii) ensure sequence quality by excluding isoforms with stop codons or frameshifts, as only fully annotated proteins from complete genomes were chosen . The final set of 30 thus reflects a balanced, non-redundant sampling strategy designed to investigate both ancient duplications (e.g., the three vertebrate classes) and more recent functional diversification within specific subfamilies .
     From these, Expasy’s "Decrease Redundancy" tool removed 10 sequences because they exceeded a 90% sequence identity threshold, keeping only 20 non‑redundant representatives. This reduction eliminates sampling bias, improves alignment and computational efficiency, and ensures that subsequent phylogenetic analysis focuses on evolutionarily informative divergence rather than nearly identical copies.
   - Curated dataset filename: myFamilyCofilin(curated).fasta

2. Multiple Sequence Alignment:
   - Tool used: MAFFT v7.526 (https://mafft.cbrc.jp/alignment/software), using the interactive MAFFT window
   - Method: The curated protein sequences were uploaded into MAFFT and aligned manually using the builtin protein alignemnt options
   - Input file: myFamilyCofilin(curated).fasta
   - Output file: MSA OUTPUT.txt
   - Output Format: Fasta Format/ Sorted (@3)
   - Strategy: G-INS-i(accurate) (@4) [Reason For Choice: greater accuracy]
   - Settings: The alignement was performed using the accurate alignnment mode where the following arguments were made:
      * global pairwise alignment --globalpair
      * iterative refinement enable --maxiterate 16
      * rearranging sequences in the final output --reorder
      * retaining gaps in final alignment --leavegappyregion [Reason For Choice: since gaps represent insertions or deletions during evolution they help preserve the true positional relationship between the residues]
   - Output File: Results were downloaded as MSA OUTPUT which was later renamed as myFamilyCofilin_aligned.fasta

3. Alignment Trimming:
   - Tool: ClipKit, executed from Python terminal in Google Colab
   - Method: The raw MSA was imported into Google Colab and processed with ClipKit to remove low quality and highly gapped positions.
   - Reason: Trimming improved the reliability of the alignment by retaining only informative sites for downstream phylogenetic inference.
   - Input file: myFamilyCofilin_aligned.fasta
   - Command used: !python -m clipkit {simple_name} -o trimmed.fasta -m smart-gap
   - What was done:
     The trimming removed two main categories of regions. First, large N‑terminal and C‑terminal extensions unique to a few sequences—such as the long N‑terminal tail in Chaetomium , chicken (NP_001020250.1), and the multidomain protein 1SQC_1—were excised because they aligned only to gaps in most other sequences. Second, internal low‑complexity or repeat‑rich inserts, notably the poly‑lysine/asparagine repeats in Plasmodium (XP_001350375.1) and the repetitive dipeptide runs in Tetrahymena (P10949.1), were trimmed as they created blocks where a single sequence carried >50% gaps relative to the rest. By removing these hypervariable, insertion‑only columns, the resulting trimmed alignment eliminated all positions with >50% gaps, thereby converting a gap‑dominated, phylogenetically unreliable alignment into a compact, information‑rich block where every column contains data from the majority of sequences. This resolves the problem by ensuring that downstream analyses (e.g., distance calculations or tree building) are no longer biased by spurious or misaligned gap‑rich regions.

  4. Phylogenetic Tree Construction:
     - Tool: IQ-TREE v 2.4.0, executed in the Python environment of Google Colab
     - Method: The trimmed alignment was analysed in Google Colab using IQ-TREE to infer a maximum likelihood phylogenetic tree.
     - Command Used: !iqtree -s {fasta_name} -m TEST -B 1000 -bnni -nt AUTO -quiet
     - Parameters taken:
       * -m MFP: ModelFinder Plus (tests all models)
       * -B 1000: Ultrafast bootstrap (1000 replicates)
       * -alrt 1000: SH-aLRT branch test (1000 replicates)
       * -bnni: Optimize bootstrap trees (reduces overestimation)
       * -nt AUTO: Use all CPU cores
       * -pre phylo: Prefix for output files
       * -safe: Protect against numerical issues
    - Output: The resulting tree was exported as phylo.treefile
         Other documents like bootstrap values, log, contree were also downloaded as phylo.log, phylo.contree, bootstrap_values (all are attached)
    - The tree file was viewed by iTOL

  5. Phylogenetic Tree Analysis:
         The phylogenetic tree separates the cofilin/ADF family into three major clades. Vertebrate sequences (Destrin, cofilin-1, cofilin-2 from human, rat, chicken, and zebrafish) form one clade, indicating that gene duplications early in vertebrate evolution gave rise to functionally distinct paralogs: destrin primarily severs actin filaments, while cofilins mainly depolymerize them, with tissue-specific expression (cofilin-2 in muscle, cofilin-1 ubiquitous). Invertebrates and protists (Dictyostelium, Leishmania, C. elegans, Anopheles, Toxoplasma) form a second clade, where single-copy homologs and longer branch lengths suggest accelerated evolution linked to diverse life cycles (e.g., parasitism). Plants (Arabidopsis thaliana and two additional Arabidopsis sequences) form a distinct third clade, reflecting lineage-specific gene duplications that allow fine-tuned actin regulation in root hairs, pollen tubes, and stress responses.
Outgroup placement using Tetrahymena, Plasmodium, and coactosin (Entamoeba) confirms that cofilins originated early in eukaryotic evolution before the divergence of animals, plants, and fungi. The absence of fungal sequences from this tree suggests they would branch near protists. All sequences retain the conserved ADF-H actin-binding domain, but regulatory mechanisms differ: vertebrate cofilins have a Ser-3 phosphorylation site (inactivated by LIM kinase), while many protist and plant homologs lack this precise regulation, relying instead on pH or PIP2 binding. Longer branches for parasitic protists (Leishmania, Plasmodium, Toxoplasma) indicate accelerated evolution, likely driven by host-pathogen arms races and adaptation to intracellular actin-based motility. Thus, the tree reveals that the cofilin family evolved via ancient and lineage-specific duplications, producing paralogs with specialized actin-remodeling functions across eukaryotes.

6. LONG BRANCH ATTRACTION: 
 Long-branch attraction (LBA) is a phylogenetic artifact where rapidly evolving lineages (long branches) are incorrectly inferred as closely related due to convergent or homoplastic substitutions, rather than true shared ancestry. It occurs because high mutation rates increase the probability of multiple substitutions at the same site, erasing phylogenetic signal and causing unrelated long-branch taxa to cluster together, especially when using simpler evolutionary models that underestimate multiple hits. LBA is exacerbated by insufficient taxon sampling, unequal evolutionary rates among lineages, and model misspecification. Common mitigation approaches include:
(1) using more realistic substitution models (e.g., Gamma-distributed rates, mixture models, or site-heterogeneous models like CAT-BP)
(2) increasing taxon sampling to break long branches
(3) removing fast-evolving or saturated sites
(4) using tree‑likelihood methods that account for rate variation across sites
(5) performing data recoding (e.g., Dayhoff recoding for amino acids) to reduce homoplasy.

The tree for the Cofilin family includes several potential long-branch attraction risks. The deepest branches—Tetrahymena, Plasmodium, and Coactosin (Entamoeba)—all show notably longer branch lengths compared to most other taxa, raising concern that LBA might artificially group these three together as an outgroup clade when they may not share recent common ancestry. Similarly, Thermococcus (an archaeal sequence) and Cofilin Chaetomium (a fungus) also have elongated branches; if true, LBA could incorrectly pull them toward the root or toward each other. The placement of Plasmodium (which contains long, repetitive, low‑complexity inserts) is particularly suspect. 

## REPRODUCIBILITY INSTRUCTIONS:

1. Data Retrieval and Curation:
   - Retrieve Cofilin family sequences manually fromt Protein Data Bank [1AK6_A (Cow), 1SQC_1 (Slime mold), 7Q8S_2 (Fission yeast), 9FP8_1 (Malaria mosquito), 2MOT_1 (Arabidopsis), 4LIZ_1 (Rice), 1F7S_1 (Arabidopsis), NP_001076577.1 (Dictyostelium), XP_001350375.1 (Plasmodium), AAI66724.1 (Zebrafish), NP_001020250.1 (Chicken), NP_001192114.1 (Arabidopsis), NP_197780.1 (Arabidopsis), P10949.1 (Tetrahymena), KAI3654753.1 (Thermococcus), O14413 (Human), NP_001102452.2 (Rat), A0A024G0G8 (Leishmania), G0W6H8_O42512 (Chaetomium globosum / Zebrafish), P45596 (C. elegans)]
   - Verify Protein Identitity and classification using Swiss Prot.
   - Remove redundant sequences via SwissProt curation tools; retain 20 non-redundant representatives: 
   - Check physicochemical properties using ProtParam
   - Save curated sequences as myFamilyCofilin(curated).fasta

2. Multiple Sequence Alignment:
   - Tool: MAFFT v7.526
   - Upload myFamilyCofilin(curated).fasta to the MAFFT interactive server or run locally
   - Parameters:
     * Strategy: G-INS-i(accurate)
     * --globalpair
     * --maxiterate 16
     * -- reorder
     * -- leavegappyregion
    - Save output as myFamilyCofilin_aligned.fasta

3. Alignment Trimming:
   - Tool: ClipKit (python)
   - Run in Google Colab or local environment command: !python -m clipkit {simple_name} -o trimmed.fasta -m smart-gap
   - Save trimmed output as myFamilyCofilin_trimmed.fasta

4. Phylogenetic Tree Construction:
   - Tool: IQ-TREE v 2.4.0
   - Run in Google Colab or local environment: !python -m clipkit {simple_name} -o trimmed.fasta -m smart-gap
   - Output tree file: phylo.treefile

5. Tree Visualisation:
   For Tree Visualisation use any webs erver like iTOL any downloadable application like FigTree or any Newick Viewer.


   --------------------------------------------------------------------END OF DOCUMENTATION----------------------------------------------------------------------------
   
     
         
   
                
     




  
      




