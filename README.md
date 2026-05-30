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
     *  Redundant sequences were removed by SwissPort and only the 20 representative non duplicating sequences were retained (1AK6, 1CFY, 1SQC_1, 4BEX, 4LIZ_1, 7Q8S_2, 1F7S_1, 2MOT_1, 9FP8_1, 1TVJ)
     *  ProtParam was used to examine basic physicochemical properties and help confirm sequence consistency.
     *  The final dataset was curated to include a non-redundant set of Cofilin homologs suitable for phylogenetic analysis.
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
      * N-terminal tail (TMITPSSGNSA and equivalents): Trimmed because it is a highly variable, unconserved extension present in only some sequences.Long gap-rich region before the conserved VADE/equivalent motif: Trimmed because it corresponds to highly divergent, poorly aligning insertions/low-complexity regions.
      * Internal variable loop within the ADF-H domain (around the LPE/KDCRY region): Trimmed because it represents large, non-homologous insertions in sequences like 9FP8_1 that would otherwise force gap expansion in the core alignment.
      * C-terminal extension beyond the ADF-H domain (the entire 1SQC_1 C-terminal domain): Trimmed because it belongs to a structurally non-homologous domain not present in the other sequences, causing massive misalignment.
      * Final trailing variable tails (e.g., HH, GPEDL... etc.): Trimmed because they are non-conserved, disordered, or absent in the majority of sequences.
    - Output file: The final trimmed sequences were exported as myFamilyCofilin_trimmed.fasta.

  4. Phylogenetic Tree Construction:
     - Tool: IQ-TREE v 1.6.12, executed in the Python environment of Google Colab
     - Method: The trimmed alignment was analysed in Google Colab using IQ-TREE to infer a maximum likelihood phylogenetic tree.
     - Command Used: !iqtree -s {fasta_name} -m TEST -B 1000 -bnni -nt AUTO -quiet
     - Parameters taken:
       * -m TEST: Find best model
       * -B 1000: Ultrafast bootstrap (1000 replicates - FAST and ACCURATE)
       * -bnni: Optimize bootstrap trees (reduces overestimation)
       * -nt AUTO: Use all CPU cores
       * -quiet: Less verbose output
      - Output: The resulting tree was exported as myFamilyCofilin_tree.treefile
      - The tree file was viewed by https://etetoolkit.org/treeview/

  5. Phylogenetic Tree Analysis:
     The tree is split by a deep root into two distinct groups. Clade 1( 1AK6, 4BEX, 1CFY) is separated from Clade 2 (1F7S,9FP8,4LIZ,7Q8S,1SQC,2MOT), suggesting an ancient gene duplication event or a fundamental functional split within the family.
     - Clade 1: ADF subgroup: The goruping of 1AK6, 4BEX, and 1CFY represents the classic members of the family. This tight cluster indicates strong sequence and structural conservation consistent with the core actin-depolymerizing function.
     - Clade 2: Divergent and Specialised Members: The second major clade contains sequences that are significantly diverged, including specialised or evolutionary distant relatives:
        * 9FP8 and 4LIZ cluster together. They are not typical cofilins but are rather related to the twinfilin family, which share an ADF-H domain but have a completely different biological role.
        * 1F7S,7Q8S,1SQC and 2MOT form a separate, highly divergent branch. They posses the ancestral ADF-H fold but have potentially evolved distinct regulatory elements or substrate-binding surfaces not found in the classic cofilins
       - The deep branching of Clade 2 makes the classic cofilins (1AK6 cluster) appear as a derived, monophyletuc cubgroup within a larger, more functionally diverse ADF-H domain superfamily. The tree correctly separates the actin-severing cofilins from the non-severing, domain related proteins twinfilins.


## REPRODUCIBILITY INSTRUCTIONS:

1. Data Retrieval and Curation:
   - Retrieve Cofilin family sequences manually fromt Protein Data Bank.
   - Verify Protein Identitity and classification using Swiss Prot.
   - Remove redundant sequences via SwissProt curation tools; retain 10 non-redundant representatives: 1AK6, 1CFY, 1SQC_1, 4BEX, 4LIZ_1, 7Q8S_2, 1F7S_1, 2MOT_1, 9FP8_1, 1TVJ
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
   - Tool: IQ-TREE v1.6.12
   - Run in Google Colab or local environment: !python -m clipkit {simple_name} -o trimmed.fasta -m smart-gap
   - Output tree file: myFamilyCofilin_tree.treefile

5. Tree Visualisation:
   For Tree Visualisation use any webs erver like https://etetoolkit.org/treeview/ or any application like FigTree or any Newick Viewer.


   --------------------------------------------------------------------END OF DOCUMENTATION----------------------------------------------------------------------------
   
     
         
   
                
     




  
      




