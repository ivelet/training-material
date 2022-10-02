---
layout: tutorial_hands_on

title: Single Cell Multi Omics Factorial Analysis with Muon
Questions:
- How to create multimodal data to combine multiple modalities?
- How to perform multi-omics factorial analysis for single-cell RNA-seq and ATAC-seq integration? 
- How to cluster multimodal data? 
- How to rank and list differentially expressed genes and differentially accessible peaks jointly with multimodal data? 
- How can cell type annotations be assigned to multimodal data? 
- How to visualize marker genes and peaks for multimodal data?

Objectives:
- Describe the MuData object to store data with multiple modalities 
- Combine multiple individual modalities into a single multimodal data object 
- Perform multi-omics factorial analysis and integration 
- Perform clustering of multiple modalities 
- Rank and list differentially expressed genes and differentially expressed peaks in integrated RNA and ATAC data 
- Assign cell type labels and annotations to multimodal data 
- Produce UMAP plots for multimodal data 
- Visualize marker genes and peaks for multimodal data

time_estimation: 3H

key_points:
- The take-home messages
- They will appear at the end of the tutorial
contributors:
- contributor1
- contributor2

---


# Introduction

[//]: # ({:.no_toc})
Advances in single-cell genomics technologies have enabled the investigation of gene regulation of 
multicellular organisms at unprecedented resolution and scale. The addition of combining multiple modalities, single-cell
multimodal analysis is another major step toward understanding the inner workings of biological systems. 

In this tutorial, we investigate multimodal factor analysis for RNA-seq and ATAC-seq modalities and how to perform 
mutli-omics factor analysis and integration of these two modalities using the Python library, muon, which is heavily based
on the similar library, ScanPy.

We investigate how to combine multiple modalities and then 
perform multi-omics factorial analysis and clustering on an integration of them. This includes ranking genes and 
peaks, and assigning annotations and visualizing marker genes and peaks. This is illustrated using RNA and 
ATAC datasets after they have undergone scRNA-seq and ATAC-seq respectively.

<!-- This is a comment. -->

> ### Agenda
>
> In this tutorial, we will cover:
>
> 1. Data
>    1. MuData
>    2. Create MuData: Combining Modalities 
> 2. Comparing Cell Type Annotations
> 3. Multi-omics Factor Analysis
> 4. Clustering using MOFA Embeddings
> 5. Ranking Differentially Expressed Genes and Accessible Peaks
> 6. Cell Type Annotation
> 7. Visualizing Marker Genes and Peaks
{: .agenda}

# Data
In this tutorial, we analyze data of two separate modalities: RNA and ATAC. The data for each of these respecive modalities 
is derived from the following PBMC data. The data for each modality must then be processed and obtained via the respective workflows:
1. Gene Expression Processing (based on processing anc clustering PBMCs) 
2. Peaks Processing (follows flow of previous one but introduces ATAC-related functionality for data processing and visualization)

We will fist take a look at how we create and handle multimodal data using the data object MuData from muon. 
This workflow and tutorial is explained using two h5ad files for the RNA and ATAC modality respectively. These datasets are obtained 
through the listed raw data files and then performing the two workflows respectively as listed previously.

If you would like to continue this tutorial with the same modality files, please run both workflows to obtain them before continuing.


## MuData
MuData is a format for annotated multimodal datasets. MuData is native to Python but provides cross-language functionality via HDF5-based .h5mu files.
mudata package introduces multimodal data objects (mudata.MuData class) allowing Python users to work with increasigly 
complex datasets efficiently and to build new workflows and computational tools around it. MuData objects enable 
multimodal information to be stored & accessed naturally, embrace AnnData for the individual modalities, and can be 
serialized to .h5mu files. Learn more about multimodal objects as well as file formats for storing & sharing them. (LINK)

Muon is a flagship framework for multimodal omics analysis. This is the framework we use in this tutorial to perform 
multimodal analysis and this framework has been built around the MuData format. 


## Creating MuData: Combining Modalities 
We will now create our multimodal data object we will need for single cell multimodal analysis. We will do this by 
combining two modalities which are stored as the files 'rna.h5ad' and 'atac.h5ad'. Note that these two files can be directly
read and output as AnnData objects which MuData is built on. Thus we can view MuData as a collection of AnnData objects.


> ### {% icon hands_on %} Hands-on: Create Multimodal Data from Individual Modalities
>
> 1. Create a new history for this tutorial
> 2. Run the listed workflows to obtain the RNA and ATAC datasets as .h5ad files
> 3. Rename the datasets to easily identify the RNA and ATAC modalities: 'rna.h5ad' and 'atac.h5ad'
> 4. Check that the datatype of both files is in the correct AnnData (.h5ad) format
> 5. {% tool [Create MuData from Annd](mudata_import) %} with the following parameters:
>    - *"First Modality"*: `RNA`
>    - *"RNA Modality Name"*: `rna`
>    - *"RNA h5ad Matrix"*: `RNA h5ad file`
>    - *"Second Modality"*: `ATAC`
>    - *"RNA Modality Name"*: `atac`
>    - *"RNA h5ad Matrix"*: `ATAC h5ad file`
> 6. Rename the generated `.h5mu` file to `MuData`
>    
>
{: .hands_on}

This tool combines modalities from both files and assigns the names specified for them. 
It also automatically performs some basic quality control using 'muon.pp.intersect_obs()'. 
This ensures that only the cells that are present in both modalities are retained in the MuData file.

# Comparing Cell Type Annotations
Now that we have combined our two modalities and performed some quality control so that only the cells present for both 
modalities were retained in our new multimodal dataset. Before performing the integration, we can compare how the cell 
type annotations in individual modalities match each other. We can do this by calculating a general score that compares 
those clustering solutions. One such score that we will use here is the 
[the adjusted Rand index](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.adjusted_rand_score.html). 
The Rand index computes a similarity measure between two clusterings by considering all pairs of samples and counting 
pairs that are assigned in the same or different clusters in the predicted and true clusterings.
The Adjusted Rand Index then adjusts this for chance into the ARI score (refer to detail). These results can then be visualized
using a heatmap.

> ### {% icon details %} Adjusted Rand Index 
>1. `ARI = (RI - Expected_RI) / (max(RI) - Expected_RI)`
>2. The adjusted Rand index is thus ensured to have a value close to 0.0 for random labeling independently of the number of clusters and samples and exactly 1.0 when the clusterings are identical (up to a permutation).
>3. ARI is a symmetric measure:
>4. `adjusted_rand_score(a, b) == adjusted_rand_score(b, a)`
> 
> ### {% icon hands_on %} Hands-on: Compare Cell Type Annotations for Multimodal Data 
>
> 1. {% tool [Compare cell type annotations](compare_annotations) %} with the following parameters:
>     - *"RNA ATAC MuData Object"*: MuData (h5mu) file created from combining individual modalities in previous step 
>     - *"Name of the first modality"*: `rna`
>     - *"Name of the second modality"*: `atac`
>     - *"Cell type annotation of first modality"*: `rna:celltype`
>     - *"Cell type annotation of first modality"*: `atac:celltype`
>     - *"Components to plot"*: `1,2 . 3,4`
> 2. Click on the display icon of the output `.png` file showing the heatmap results to observe this 
>
{: .hands_on}

> ### {% icon question %} Questions
>
> 1. What can you observe from the heatmap regarding the t?
>
> > ### {% icon solution %} Solution
> >
> > 1. t seems that cell types are highly reproducible across modalities with the only exception of the 
CD14/intermediate monocytes annotation that doesn't seem particularly confident — most probably in the ATAC modality
> >
> {: .solution}
>
{: .question}

# Multi-omics Factor Analysis 

Multi-omics Factor Analyis (MOFA) is a factor analysis model that provides a general framework for the integration of 
multi-omic data sets in an unsupervised fashion. Intuitively, it can be viewed as a versatile and statistically rigorous 
generalization of principal component analysis to multi-omics data.


![Alternative text](../../images/image_name "Legend of the image")

> ### {% icon details %} More details about MOFA
>
> Given several data matrices with measurements of multiple -omics data types on the same or on overlapping sets of samples, MOFA infers an interpretable low-dimensional 
representation in terms of a few latent factors. These learnt factors represent the driving sources of variation across 
data modalities, thus facilitating the identification of cellular states or disease subgroups. 
You may find out more from the official [MOFA website](https://biofam.github.io/MOFA2/)
> 
> For more details, read the following papers by MOFA:
>    - general framework: [published in Molecular Systems Biology](http://msb.embopress.org/cgi/doi/10.15252/msb.20178124)
>    - multi-group framework and single cell applications: [MOFA+, published in Genome Biology](http://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02015-1)
>    - temporal or spatial data: [MEFISTO, published in Nature Methods](https://www.nature.com/articles/s41592-021-01343-9)
{: .details}


> ### {% icon hands_on %} Hands-on: Perform Multi-omics Factor Analysis 
>
> 1. {% tool [Multi Omics Factorial Analysis with Muon](mofa) %} with the following parameters:
>     - *"RNA ATAC Multimodal Data Matrix"*: `MuData (h5mu) file from previous step`
>     - *"Name of the first modality"*: `rna`
>     - *"Name of the second modality"*: `atac`
>     - *"Load a trained mofax model?"*: `No`
>     - *"Number of factors to train the MOFA model with"*: `10`
>     - *"Name of the color for each modality (Plotting)"*: `celltype_colors`
>     - *"Cell type annotation color of first modality (Plotting)"*: `celltype`
>     - *"Cell type annotation name of second modality (Plotting)"*: `celltype`
>     - *"Components to plot (Plotting)"*: `1,2 . 3,4`
{: .hands_on}

As you can observe, this tool produces 4 output files. The first is a MuData object as a H5MU file. This is our original 
file with the MOFA embeddings added. This is then used in the tool to produce plots directly using muon.pl.muon(). The tool 
also produces a UMAP plot using ScanPy and two plots of the respective modalities from muon.pl.mofa(), which allows both 
modalities to be jointly plotted after performing multi-omics factor analysis. Finally the model that we have trained is 
saved and returned. This can be used the next time we use this tool to save time so that the same model does not need to 
trained from scratch. 

# Cluster MOFA Embeddings
After the training, the embedding will be added to the `obsm`slot of the `MuData`.
The produced `h5mu` MuData file produced from the MOFA tool in the last step will be the input file with the added 
embeddings from the multiomics factor analysis added to the `obsm` slot of the MuData. These embeddings are very useful 
now and can be used directly for plotting using both muon and scanpy as it contains both `.obs` and `.obsm` as needed for 
both libraries of plotting functions.

We can now perform conventional clustering on the MOFA embeddings and can also visualize them on a UMAP. The individual 
features from modalities can also be used when plotting embeddings and if we wish to generate custom plots using 
matplotlib and seaborn libraries and tools based on these.

> ### {% icon hands_on %} Hands-on: Perform Clustering on Multimodal Data using MOFA Embeddings
>
> 1. {% tool [Cluster MOFA Embeddings](cluster_mofa_embeddings_with_mudata) %} with the following parameters:
>    - *"RNA ATAC with MOFA Embeddings"*: `MuData (h5mu) file after performing MOFA`
>    - *"Clustering method to use"*: `Cluster cells into subgroups using 'scanpy.tl.leiden'`
>    - *"Courseness of the clustering"*: `1.0`
>    - *"Random State"*: `0`
>    - *"Key under which to add the cluster labels"*: `leiden_joint`
>    - *"Use weights from KNN graph?"*: `Yes`
>    - *"Number of iterations of te Leiden clustering algorithms to perform*": `-1`
>    - *"Location of legend*": `on data`
>    - *"Keys for annotations of observations/cells or variables/genes*": `KLF4, chr9:107480158-107492721`
>
{: .hands_on}

# Ranking Differentially Expressed Genes and Accessible Peaks in Multimodal Data
We can now define cell types jointly based on both modalities using ScanPy and muon. Now that we have performed clustering, 
we will rank genes and peaks from each of our respective modalities. Then we will use this to list differentially expressed 
genes from our RNA modality and differentially accessible peaks from our ATAC modality. This will be produced as a table
listing all these (.csv file).

> ### {% icon hands_on %} Hands-on: Rank and List Differentially Expressed Genes and Differentially Accessible Peaks 
>
> 1. {% tool [Rank and List Genes and Peaks in Multimodal Data with Scanpy](rank_genes_and_peaks_muon) %} with the following parameters:
>    - *"RNA ATAC Clustered Multimodal Data"*: `MuData (h5mu) file after clustering`
>    - *"The key of the observations grouping to consider"*: `leiden_joint`
>    - *"Method"*: `t-test_overestim_var`
>    - *"The name of the RNA modality in MuData object"*: `rna`
>    - *"The name of the ATAC modality in MuData object"*: `atac`
>    - *"Maximum number of columns in list"*: `200`
{: .hands_on}

# Assigning Cell Type Labels
Having studied the markers of individual clusters, we can add a new cell type annotation to the object. 

We will add new cluster names to the object and then also re-order the categories for the next plot. Finally, we take 
the colours from a palette and present the results on a UMAP plot.

> ### {% icon hands_on %} Hands-on: Assign New Cell-Type Labels 
>
> 1. {% tool [Assign Cell Type Labels for MuData](assign_cell_type_labels_mudata) %} with the following parameters:
>    - *"RNA ATAC Data Matrix"*: `RNA ATAC with Ranked Genes and Peaks`
>    - *"New Cluster Names Keys"*: `0,8,22,1,6,17,2,5,20,10,11,19,12,16,15,3,7,9,14,4,13,18,21`
>    - *"New Cluster Names Values"*: `CD4+ naïve T, CD4+ naïve T, CD4+ naïve T, CD4+ memory T, CD4+ memory T, CD4+ memory T, CD8+ naïve T, CD8+ naïve T, CD8+ naïve T, CD8+ cytotoxic effector T, CD8+ transitional effector T, MAIT, NK, naïve B, memory B, classical mono, classical mono, classical mono, classical mono, intermediate mono, non-classical mono, mDC, pDC`
>    - *"Categories to reorder"*: `CD4+ naïve T,CD4+ memory T,CD8+ naïve T, CD8+ transitional effector T, CD8+ cytotoxic effector T,MAIT, NK,naïve B, memory B,classical mono,intermediate mono, non-classical mono,mDC, pDC`
>    - *"Name of the colormap instance"*: `rainbow`
>    - *"Keys for annotations of observations/cells or variables/genes"*: `celltype`
>    - *"Location of legend"*: `on data`
>    - *"Draw a frame around the scatter plot"*: `Yes`
{: .hands_on}

# Visualize Markers 
Finally, we will visualize some marker genes across cell types and marker peaks. We will then produce dotplots for the 
marker genes and marker peaks defined in each modality respectively.

> ### {% icon hands_on %} Hands-on: Visualize Marker Genes and Marker Peaks in Multimodal Data 
>
> 1. {% tool [Visualize Markers with MuData](visualise_markers_mudata) %} with the following parameters:
>    - *"RNA ATAC Data Matrix"*: `RNA ATAC with Marker Genes and Peaks`
>    - *"Name of the RNA modality"*: `rna`
>    - *"Name of the ATAC modality"*: `atac`
>    - *"The key of the observation grouping to consider."*: `leiden_joint`
>    - *"Marker genes"*: `IL7R, TRAC, GATA3, LEF1, FHIT, RORA, ITGB1, CD8A, CD8B, CD248, CCL5, KLRB1, SLC4A10, IL32, GNLY, NKG7, CD79A, MS4A1, IGHD, IGHM, IL4R, TNFRSF13C, KLF4, LYZ, S100A8, ITGAM, CD14, DPYD, ITGAM, FCGR3A, MS4A7, CST3, CLEC10A, IRF8, TCF4`
>    - *"Marker peaks"*: `chr14:99255246-99275454, chr10:33135632-33141841, chr1:1210271-1220028, chr2:86783559-86792275, chr12:10552886-10555668, chr11:114072228-114076352, chr5:150385442-150415310, chr22:41931503-41942227, chr22:41917087-41929835, chr6:167111604-167115345, chr9:107480158-107492721, chr5:1476663-1483241, chr10:75399596-75404660, chr1:220876295-220883526, chr17:81425658-81431769, chr7:98641522-98642532`
> 2. Observe the following two plots produced of the marker genes and marker peaks respectively
{: .hands_on}


# Conclusion
In this tutorial, we investigated single-cell multimodal data analysis. In particular, we used the library muon to perform 
integrate RNA-seq and ATAC-seq data using muon then performed multi-omics factor analysis and clustering.

This workflow used was typical for multimodal single-cell gene expression and chromatin accessibility analysis:
1. Combine modalities to create multimodal data
2. Compare cell-type annotations
2. Perform multi-omics factor analysis
3. Perform clustering on MOFA embeddings
4. Rank and list differentially expressed genes and accessible peaks
5. Cell-type annotation
6. Visualize marker genes and peaks