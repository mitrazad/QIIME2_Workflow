# Microbiome data analysis using QIIME 2

**Project:** The oral microbiome of healthy and EOTRH-affected horses.

## Project overview
This repository contains a complete, reproducible microbiome analysis workflow for paired-end 16S rRNA gene amplicon sequencing data, implemented using QIIME 2 (v2025.7) and its Python API.

The project investigates the equine oral microbiome with a focus on Equine Odontoclastic Tooth Resorption and Hypercementosis (EOTRH). Samples originate from different oral niches (gum and plaque) and disease states (healthy, onset, diseased), and include both negative and positive controls.

All analysis steps are implemented and documented in a single Jupyter notebook `main.ipynb`, with intermediate artifacts and visualizations saved to disk for transparency, reproducibility, and downstream interpretation.

## Research questions
The workflow was designed to address the following research questions:
- How different is the oral microbiome between healthy and EOTRH-sick horses?
- Do microbial communities differ between gums and plaque?
- How does the horse oral microbiome compare to the human oral microbiome (literature)?
- How many microorganisms remain unclassified in horses?
- Can therapeutic recommendations be made based on the observed taxa?

The mapping between analysis steps and research questions is explained in the notebook and summarized below.

## Repository structure
```sql
project/
├── raw/                        # Demultiplexed paired-end FASTQ files
│   ├── <sample>_1.fastq.gz
│   └── <sample>_2.fastq.gz
│
├── metadata/
│   ├── sample-metadata.tsv    # Sample metadata (role, disease-state, type, etc.)
│   ├── manifest.tsv           # Generated manifest for QIIME 2 import
│   └── sample-metadata.qzv    # Metadata summary
│
├── ref/
│   └── silva-138-99-nb-classifier.qza   # Pre-trained SILVA classifier
│
├── qiime2/                     # QIIME 2 artifacts and visualizations
│   ├── diversity-asv/
│   ├── diversity-otu/
│   ├── *.qza
│   └── *.qzv
│
├── notebook/
│   └── main.ipynb
│
└── README.md
```

## Software and versions
- QIIME 2: 2025.7.0
- Plugins used:
    - demux
    - cutadapt
    - dada2
    - feature-table
    - quality-control (decontam)
    - vsearch
    - feature-classifier
    - taxa
    - phylogeny
    - diversity
    - composition (ANCOM-BC)
- Reference database:
    - SILVA 138 (99% OTUs), Naive Bayes classifier, which was downloaded from [QIIME2].(https://library.qiime2.org/data-resources)
- Python: 3.10.14

## Analysis workflow

The notebook follows a stepwise, narrative workflow, where each step includes:
- a short explanation,
- executable code,
- saved intermediate outputs.

### Main steps
**1. Metadata inspection:** Validation of required metadata fields (role, disease-state, type).

**2. Manifest creation and import:** Generation of a manifest file linking sample IDs to FASTQ files and import into QIIME 2.

**3. Quality control:** Demultiplexing summaries and inspection of read quality profiles.

**4. Primer trimming:** Removal of locus-specific primers (341F/805R) using `cutadapt`.

**5. DADA2 denoising (ASVs):** Error correction, chimera removal, and paired-end merging.

**6. Intermediate summaries:** Feature table and representative sequence summaries.

**7. Decontamination:** Prevalence-based contaminant identification using negative controls.

**8. Low-abundance filtering:** Removal of rare features after decontamination.

**9. OTU clustering (97% de novo):** Parallel OTU-based analysis for comparison with ASVs.

**10. Taxonomic assignment and barplots:** SILVA-based taxonomy for ASVs and OTUs.

**11. Phylogenetic tree construction:** MAFFT alignment and FastTree inference for phylogeny-aware metrics.

**12. Removal of controls for diversity analyses:** Only biological samples retained.

**13. Core diversity metrics:** Alpha diversity (Shannon, Faith’s PD) and beta diversity (Bray–Curtis, weighted and unweighted UniFrac).

**14. Alpha rarefaction:** Evaluation of sequencing depth sufficiency.

**15. Group significance testing:** Alpha diversity group comparisons and PERMANOVA for beta diversity.

**16. Identification of unclassified taxa:** Inspection of unassigned and genus-level unresolved features.

**17. Differential abundance analysis (ANCOM-BC):** Disease-associated taxa identified at ASV, OTU, and genus level.

## How to run the analysis
1. Place raw FASTQ files in `raw/`
2. Provide `sample-metadata.tsv` in `metadata/`
3. Ensure the SILVA classifier is present in `ref/`
4. Open and run `main.ipynb` in QIIME 2 environment

## Acknowledgements
This project was developed as part of the "Fallstudie" (Case Study) course in the Master's study program "Bio Data Science" at the University of Applied Sciences Wiener Neustadt (WS25/26). Workflow design is inspired by official QIIME 2 tutorials and course-provided example notebooks from Dmitrij Turaev.
