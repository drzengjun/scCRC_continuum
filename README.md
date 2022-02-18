# scCRC_continuum

Repository to host code produced for "Single-cell analyses reveal a continuum of cell state and composition changes in the malignant transformation of polyps to colorectal cancer" by Becker*, Nevins*, et al 2021. The peer-revieved article is available at (ADD LINK WHEN PUBLISHED) and the preprint is available at https://www.biorxiv.org/content/10.1101/2021.03.24.436532v1.  

![image](https://user-images.githubusercontent.com/15204322/147711771-0a5e3292-095c-443a-a0ef-9d8bf7afb181.png)

## Files/Data: ## 
A metadata file is included here that contains primarily the sample specific information included in the supplemental tables as well as some grouping used for calling differential genes/peaks.

Seurat objects can be found here: https://drive.google.com/drive/folders/12j9ufV1L0uWbUlab-VoXRznDLKDO7PQ_?usp=sharing. This includes the objects for the immune, stromal, and epithelail compartments, which contain annotated cells from the three compartments with likely doublet and low quality clusters removed. (We may change the location of these files in the future if we find a better place to host them, but will update this page with the location.)  

The raw data for unaffacted, polyp, and CRC samples will be hosted on the the HTAN data portal (https://data.humantumoratlas.org/) under the PRE-CANCER ATLAS: FAMILIAL ADENOMATOUS POLYPOSIS project. The raw data for normal colon samples will be hosted on the HuBMAP data portal (https://portal.hubmapconsortium.org/) under the Stanford TMC (https://portal.hubmapconsortium.org/search?group_name[0]=Stanford%20TMC&entity_type[0]=Dataset).  

A metadata file is included in this repository that contains how we named the samples in the ATAC and RNA projects and the corresponding metadata: hubmap_htan_metadata_atac_and_rna_final.csv.  

## Scripts: ##
Note that many of the scripts use the same functions. Here, necessary functions were copied into each individual file for simplicity. 

#### Preprocessing with cell ranger ####  
cell_ranger_sbatch_scripts contains run_cell_ranger_ATAC_example.sbatch and run_cell_ranger_RNA_example.sbatch, which are examples of how cell ranger was used to generate expression matricies and fragments files from fastqs. 

#### snRNA Analysis Scripts ####  
*scRNA_initial_clustering.R*  
-make seurat object will all cells  
-run doublet finder on individual samples
-divide into three groups (immune, epithelial, and stromal) for downstream analysis  

*snRNA_epithelial_analysis.R*  
-LSI dimensionality reduction and clustering of normal epithelial cells  
-projection of unaffected, polyp, and CRC epithelial cells into normal manifold  
-computation of differentials and malignancy continuum  

*snRNA_stromal_analysis.R*  
-Standard Seurat 3 (Stuart et al. 2019) workflow for dimensionality reduction and clustering of stomal cells from all samples. The seurat object linked to in the data section is what was produced from this script. 

snRNA_immune_analysis.R  
-Standard Seurat 3 (Stuart et al. 2019) workflow for dimensionality reduction and clustering of stomal cells from all samples

#### scATAC Analysis Scripts ####  
ArchR is utilized for a significant portion of the scATAC analysis. You can find additional details on ArchR here (https://www.archrproject.com/).

*scATAC_initial_clustering.R*  
-Initial clustering of all scATAC cells with ArchR (Granja et al. 2021)
-This first creates arrow files starting from the fragments files generated by cell ranger. You can genetae the fragments files from the fastqs or downlead them directly. 
-Note that some aspects of the analysis are non-determenistic (https://www.archrproject.com/bookdown/iterative-latent-semantic-indexing-lsi.html). We have provided lists of cells that were obtained after our doublet filtering as well as the list of cells included in each category to help make steps of the analysis reproducible.

*scATAC_subset_immune.R*  
-clustering and annotation of scATAC immune cells

*scATAC_subset_tcells.R*  
-clustering and annotation of scATAC t-cells

*scATAC_construct_epithelial_reference.R*  
-clustering and annotation of normal reference 

*scATAC_project_diseased_cells.R*  
-script for projecting scATAC cells into normal reference  

*scATAC_subset_epithelial.R*  
-script for calling peaks, computing malignancy continuum, and computing differentials for all scATAC epithelial cells

#### WGS Analysis Scripts ####  
-scripts are in the WGS folder. Mutect2_commandLineDirections.sh has the command line entries for running the 2 other scripts.  

**Questions/Comments:**

For comments/questions, please either add a GitHub issue or email wbecker@stanford.edu.
