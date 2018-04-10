# [Systematic morphological profiling of human gene and allele function reveals Hippo-NF𝛋B pathway connectivity](http://biorxiv.org/content/early/2016/12/08/092403.full.pdf+html) #

### This code is associated with the paper from Rohban et al., "Systematic morphological profiling of human gene and allele function via Cell Painting". eLife, 2017. http://dx.doi.org/10.7554/eLife.24060

## Abstract ##
We hypothesized that human genes and disease-associated alleles might be systematically functionally annotated using morphological profiling of cDNA constructs, via a microscopy-based Cell Painting assay. Indeed, 50% of the 220 tested genes yielded detectable morphological profiles, which grouped into biologically meaningful gene clusters consistent with known functional annotation (e.g., the RAS-RAF-MEK-ERK cascade). We used novel subpopulation-based visualization methods to interpret the morphological changes for specific clusters. This unbiased morphologic map of gene function revealed TRAF2/c-REL negative regulation of YAP1/WWTR1-responsive pathways. We confirmed this discovery of functional connectivity between the NF-𝛋B pathway and Hippo pathway effectors at the transcriptional level, thereby expanding knowledge of these two signaling pathways that critically regulate tumor initiation and progression. We make the images and raw data publicly available, providing an initial morphological map of major biological pathways for future study.

## Prerequisites ##
* Mac OS X
* R Ver. 3.2.2 (recommended version)
* MySQL Ver. 14.14 Distrib. 5.1.71 (recommended version)
* XeTeX 3.14159265-2.6-0.99996 (TeX Live 2016) (recommended version)
* Python Ver. 2.7 (recommended version, Ver. < 3 required)
* [CellProfiler-Analyst](https://github.com/CellProfiler/CellProfiler-Analyst) Ver. 2 (recommended version)
* `progressbar` Python module  

## Setup ##
* **Database setup** : Use the following commands to import the database (will be soon available in [IDR](http://idr-demo.openmicroscopy.org); `username` has to be replaced with the actual username of owner of the database :

```
echo "create database TargetAccelerator" | mysql -u username -p
mysql -u username -p TargetAccelerator < TargetAccelerator.sql
mysql -u username -p TargetAccelerator < Per_Object_View.sql
```

* **Creating profiles** : Use the script `profile.sh` in the `code/profiling` directory to create the profiles based on the database which contains the single cell data. The output would be placed in `input/profiles`. Change the host and database information in the file `input/TA_OE_B1/TargetAccelerator.properties` if needed.
* **Creating the image folder** : Transfer the image folder containing 6 plates of images from [IDR](http://idr-demo.openmicroscopy.org) to the `images` directory, such that each plate (e.g. 41744) would have a different directory under `images` whose name is the plate number (e.g. `images/41744`).  
* Run `Required_Packages_Intallation.R` to install the required packages to run the scripts.

## Usage ##
Use the following scripts in the `code` directory to analyze the data (set the working directory to `code` first):

* `Initial_analysis.Rmd` : initial processing of the profiles. This would then create a file containing the results `results/master/Initial_analysis/Initial_analysis_workspace.RData`, which would then be used in other scripts. The analyses in this file include : filtering the data based on QC metrics, median polishing, feature selection and dimensionality reduction, hit selection, z-scoring, and clustering. 
* `Dendrogram_cutoff_selection_based_on_Stability.R` : an analysis which finds the threshold used for cutting the dendrogram to obtain the clusters (Fig. 3 (without subpopulation information overlay), and Supp. Fig. 4). 
* `ORF_Sequence_Score_when_matched_to_transcripts.R` : downloads ORF sequence matching score to the transcripts (the result would be stored in `results/master/ORFs_sequence_matching_transcripts_percentage`; you need to be connected to the internal Broad Institute network for this to work.  
* `Gather_Protein_Interaction_Data.R` : downloads relevant protein interaction data from the [BioGRID](https://thebiogrid.org) and place the results in `results/master/protein_interaction_data`. 
* `Printing_out_Original_Constructs_Info.R` : prints out the set of all constructs used in the experiment (Supp. Table 1)
* `Phenotype_Strength_and_WT_Pairs_Correlation_Plot.R` : phenotype strength and WT clones correlation diagram (Fig. 2A, 2B, and Supp. Fig. 1)
* `Comparison_to_Protein_Interaction_Data.R` : testing the hypothesis that for the highly correlated genes in Cell Painting, their corresponding proteins interact more than what expected by random chance (Supp. Fig. 6, Supp. Table 3)
* `Comparison_to_Pathway_Annotation.R` : (Supp. Table 4)
* `Phenotype_Strength_across_Pathways.R` : (Supp. Fig. 2)
* `Constitutively_Active_Mutant_Comparison_to_WT.R` : (Supp. Table 2)
* `Phenotype_Strength_of_Genes_Supposed_to_Change_Morphology.R` : (Fig. 2C)
* `GO_Term_Analysis_of_Clusters.R` : (Supp. Table 5)
* `Cluster_type_A_pdfs.Rmd` : (Supp. Data for Cluster, PDFs type A, Fig. 4A and 4B type of plots are included in the PDFs. Results are stored in the `results/master/Clusters` directory)
* `Cluster_type_B_pdfs.R` : Supp. Data for Cluster, PDFs type B, Fig. 4C, 6A, 6B and Supp. Fig. 5 type of plots are included in the PDFs. Also the data in the PDFs are used as the overlay information in Fig. 3. Results are stored in the `results/master/Clusters` directory)
* `Target_Prediction_based_on_L1000_readout.R` : code used to find common up- or down-regulated genes when a set of genes are overexpressed 
* `GSEA_Plots.R` : creating gene set enrichment analysis (GSEA) plots, based on query results from LINCS (Fig. 6C, Supp. Fig. 7, and Supp. Fig. 8). 



