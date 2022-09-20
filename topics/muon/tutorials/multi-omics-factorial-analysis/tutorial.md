---
layout: tutorial_hands_on

title: Single Cell Multi Omics Factorial Analysis with Muon
zenodo_link: ''
questions:
- How to create multimodal data to combine multiple modalities?
- How to perform multi-omics factorial analysis for single-cell RNA-seq and ATAC-seq integration? 
- How to cluster multimodal data? 
- How to rank and list differentially expressed genes and differentially accessible peaks jointly with multimodal data? 
- How can cell type annotations be assigned to multimodal data? 
- How to visualize marker genes and peaks for multimodal data?

objectives:
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
>
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

Muon is a flagship framwwork for multimodal omics analysis. This is the framework we use in this tutorial to perform 
multimodal analysis and this framework has been built around the MuData format. 


## Creating MuData: Combining Modalities 
We will now create our multimodal data object we will need for single cell multimodal analysis. We will do this by 
combining two modalities which are stored as the files 'rna.h5ad' and 'atac.h5ad'. Note that these two files can be directly
read and output as AnnData objects which MuData is built on. Thus we can view MuData as a collection of AnnData objects.


> ### {% icon hands_on %} Hands-on: Create Multimodal Data from Individual Modalities
>
> 1. Create a new history for this tutorial
> 2. Run the listed workflows to obtain the RNA and ATAC datasets as .h5ad files
> 3. Rename the datasets to identify the RNA and ATAC modalities: 'rna.h5ad' and 'atac.h5ad'
> 4. Check that the datatype
> 5. {% tool [mofa](mudata_import) %} with the following parameters:
>    "First Modality": 'RNA'
>    "RNA Modality Name": 'rna'
>    "RNA h5ad Matrix": 'RNA h5ad file' 
>    "Second Modality": 'ATAC'
>    "RNA Modality Name": 'atac'
>    "RNA h5ad Matrix": 'ATAC h5ad file' 
> 6. Rename the generated file to MuData
>    
>
{: .hands_on}

This tool combines modalities from both files and assigns the names specified for them. 
It also automatically combines some basic quality control using 'muon.pp.intersect_obs()'. 
This means it only allows the cells that are present in both modalities to be retained.

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
>     "RNA ATAC MuData Object": MuData (h5mu) file created from combining individual modalities in previous step
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
multi-omic data sets in an unsupervised fashion. Intuitively, MOFA can be viewed as a versatile and statistically rigorous 
generalization of principal component analysis to multi-omics data.


![Alternative text](../../images/image_name "Legend of the image")

> ### {% icon details %} More details about MOFA
>
> Given several data matrices with measurements of multiple -omics data types on the same or on overlapping sets of samples, MOFA infers an interpretable low-dimensional 
representation in terms of a few latent factors. These learnt factors represent the driving sources of variation across 
data modalities, thus facilitating the identification of cellular states or disease subgroups. 
>FIND OUT MORE HERE FROM THE MOFA WEBSITE: https://biofam.github.io/MOFA2/
>2. For more details, read papers by MOFA:
    - general framework: [published in Molecular Systems Biology](http://msb.embopress.org/cgi/doi/10.15252/msb.20178124)
    - multi-group framework and single cell applications: [MOFA+, published in Genome Biology](http://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02015-1)
    - temporal or spatial data: [MEFISTO, published in Nature Methods](https://www.nature.com/articles/s41592-021-01343-9)
{: .details}


> ### {% icon hands_on %} Hands-on: Perform Multi-omics Factor Analysis 
>
> 1. {% tool [Multi Omics Factorial Analysis with Muon](mofa) %} with the following parameters:
>     "RNA ATAC Multimodal Data Matrix": MuData (h5mu) file from previous step
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding

# Cluster MOFA Embeddings
After the training, the embedding will be added to the `obsm`slot of the `mdata`.
The produced h5mu MuData file produced from the MOFA tool in the last step will be the input file with the added 
embeddings from the multiomics factor analysis added to the obsm slot of the MuData. These embeddings are very useful 
now and can be used directly for plotting using both muon and scanpy as it contains both .obs and .obsm as needed for 
both libraries of plotting functions.

We can now perform conventional clustering on the MOFA embeddings and can also visualize them on a UMAP. The individual 
features from modalities can also be used when plotting embeddings and if we wish to generate custom plots using 
matplotlib and seaborn libraries and tools based on these.

> ### {% icon hands_on %} Hands-on: Perform Clustering on Multimodal Data using MOFA Embeddings
>
> 1. {% tool [Rank and List Genes and Peaks in Multimodal Data with Scanpy](rank_genes_and_peaks_muon) %} with the following parameters:
>    - "RNA ATAC with MOFA Embeddings": MuData (h5mu) file after performing MOFA
>    - "Courseness of the clustering": `1.0`
>    - "Random State": `0`
>    - "Key under which to add the cluster labels": `leiden_joint`
>    - "Use weights from KNN graph?": 'Yes'
>    - "Number of iterations of te Leiden clustering algorithms to perform": `-1`
>
{: .hands_on}

# Ranking Differentially Expressed Genes and Accessible Peaks in Multimodal Data
We can now define cell types based on both our omics using scanpy and muon. Now that we have performed clustering, 
we will rank genes and peaks from each of our respective modalities.

> ### {% icon hands_on %} Hands-on: Rank and List Differentially Expressed Genes and Differentially Accessible Peaks 
>
> 1. {% tool [Rank and List Genes and Peaks in Multimodal Data with Scanpy](rank_genes_and_peaks_muon) %} with the following parameters:
>    - "RNA ATAC with MOFA Embeddings": MuData (h5mu) file after after clustering
{: .hands_on}

# Assigning Cell Type Labels
Having studied the markers of individual clusters, we can add a new cell type annotation to the object. 

We will add new cluster names to the object and then also re-order the categories for the next plot. Finally, we take 
the colours from a paelette and plot the results on a UMAP plot.

> ### {% icon hands_on %} Hands-on: Assign New Cell-Type Labels 
>
> 1. {% tool [Assign Cell Type Labels for MuData](assign_cell_type_labels_mudata) %} with the following parameters:
>    - "RNA ATAC Data Matrix": MuData (h5mu) file after after ranking
>    - TODO: Add after adding field to define comma-separated cluster names to add 
{: .hands_on}

# Visualize Markers 
Finally, we will visualize some marker genes across cell types and marker peaks. We will then produce dotplots for each 
of these to observe the marker genes and peaks for each respective modality.

> ### {% icon hands_on %} Hands-on: Visualize Marker Genes and Marker Peaks in Multimodal Data 
>
> 1. {% tool [Visualize Markers with MuData](visualise_markers_mudata) %} with the following parameters:
>    - "RNA ATAC Data Matrix": MuData (h5mu) file after after assigning cell type labels
>    - TODO: Add after adding field to define comma-separated cluster names to add 
> 2. Observe the following two plots produced of the marker genes and marker peaks respectively
{: .hands_on}


# Conclusion
{:.no_toc}
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