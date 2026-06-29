# Lotus microbiome and population-genomic analysis workflow

This repository contains the microbiome preprocessing, validation analyses, and population-genomic visualization scripts used in the manuscript.

The workflow is organized into five main scripts:

## 01 - Microbiome traits for GWAS
Starts from the processed QIIME2/VSEARCH feature table (`table_99.qza`) and sample metadata (`metadataRep.txt`).

This script:
- filters low-depth samples and low-prevalence taxa
- rarefies the microbiome count table
- exports the rarefied OTU table used for GWAS trait generation
- computes ordinations (NMDS and MDS)
- exports replicate-specific ordination tables used as microbiome-derived phenotypes

Main output:
- `phy_rarefied_GWAS.csv`
- `rep1Nova_MDS.csv`
- `sub2Nova_MDS.csv`
- `rep3Nova_MDS.csv`
- optional NMDS exports and exploratory plots

## 02 - Microbiome validation analyses
Runs the downstream microbiome analyses used for the functional validation experiments.

This script covers:
- SynCom LORE1 mutant analysis
- Utobara natural-soil LORE1 mutant analysis
- Utobara natural haplotype analysis
- combined LORE1 + haplotype comparison

Analyses include:
- MDS ordination
- PERMANOVA and pairwise permutation MANOVA
- ANCOM-BC differential abundance
- relative-abundance summaries and figure-oriented plots

Required input files include:
- `dada2_LjRootnames_SynCom_LORE1.csv`
- `metadata_SC_LORE1.csv`
- `otu_table_Utobara_LORE1.csv`
- `metadata_Utobara_LORE1.csv`
- `otu_table_Utobara_Haplotype.csv`
- `metadata_Utobara_Haplotype.csv`
- `metadata_combined.csv`

## 03 - Japan maps
Generates the Japan maps used to visualize:
- all accessions
- North/South population structure
- ROOMIE1 haplotype distributions

Required input:
- `LotusSNP_accessions_location.csv`

## 04 - Compute snp metrics
Computes SNP-level population-genomic metrics from the genotype matrix and accession metadata.

For each SNP category, this script calculates:
- `MAF` (minor allele frequency)
- `FST_pop` (North vs South)
- `FST_founder` (Tsushima-proximal vs Mainland)
- `AUC_LOESS` (normalized area under the local LD-decay curve)

Required input:
- `Lj_accessions_SNPs.csv`
- `LotusSNP_accessions_location.csv`
- a SNP category input table containing at least `snp_id` (or `focal_SNP`) and `label`

Main output:
- `SNP_categories_all_metrics.csv`

## 05 - Genomic signatures
Reads `SNP_categories_all_metrics.csv` and generates the population-genomic plots used in the manuscript.

This script includes:
- scatter plots of MAF versus `FST_pop`
- coloring by `AUC_LOESS` or `FST_founder`
- SNP-by-accession heatmaps for the main SNP categories

Required input:
- `SNP_categories_all_metrics.csv`
- `Lj_accessions_SNPs.csv`
- `LotusSNP_accessions_location.csv`

---

## Recommended running order

Run the scripts in the following order:

1. `01_prepare_microbiome_data.R`
2. `02_microbiome_validation_analyses.R`
3. `03_japan_maps.R`
4. `04_compute_snp_metrics.R`
5. `05_plot_population_genomic_signatures.R`

Scripts 01–03 reproduce the microbiome preprocessing and validation analyses.  
Scripts 04–05 reproduce the SNP-level population-genomic metrics and visualizations.

---

## Notes

- Reproduction in this repository starts from processed count tables, genotype tables, and metadata files, not from raw sequencing reads.
- GWAS implementation, permutation framework, and clumping are maintained in a separate dedicated GWAS repository.
