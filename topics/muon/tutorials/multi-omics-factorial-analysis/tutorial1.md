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
2. Single-cell multimodal omics measurement offers opportunities for gaining holistic views of cells one by one
3. Advances in single-cell genomics technologies have enabled investigation of the gene regulation programs of multicellular organisms at unprecedented resolution and scale
4. Development of single-cell multimodal omics tools is another major step toward understanding the inner workings of biological systems
5. In this tutorial, we investigate multimodal factorial analysis for RNA-seq and ATAC-seq modalities. We also look at how to combine multiple modalities into a single data object to then use this for integration as well as other techniques on multimodal data such as clustering, ranking and listing genes and peaks, assigning cell type annotations and visualizing marker genes and peaks.
6. We will investigate how to combine multiple modalities and then perform multi-omics factorial analysis and clustering on an integration of them, including ranking and listing genes and peaks, assigning annotations and visualizing marker genes and peaks, using muon (). It will be illustrated using RNA and ATAC datasets after they have undergone scRNA-seq and ATAC-seq respectively.

<!-- This is a comment. -->

General introduction about the topic and then an introduction of the

tutorial (the questions and the objectives). It is nice also to have a

scheme to sum up the pipeline used during the tutorial. The idea is to

give to trainees insight into the content of the tutorial and the (theoretical

and technical) key concepts they will learn.


You may want to cite some publications; this can be done by adding citations to the

bibliography file (`tutorial.bib` file next to your `tutorial.md` file). These citations

must be in bibtex format. If you have the DOI for the paper you wish to cite, you can

get the corresponding bibtex entry using [doi2bib.org](https://doi2bib.org).


With the example you will find in the `tutorial.bib` file, you can add a citation to

this article here in your tutorial like this:

{% raw %} `{% cite Batut2018 %}`{% endraw %}.

This will be rendered like this: {% cite Batut2018 %}, and links to a

[bibliography section](#bibliography) which will automatically be created at the end of the

tutorial.



**Please follow our

[tutorial to learn how to fill the Markdown]({{ site.baseurl }}/topics/contributing/tutorials/create-new-tutorial-content/tutorial.html)**

> ### Agenda
>
> In this tutorial, we will cover:
>
> 1. TOC
> {:toc}
>
{: .agenda}

# Title for your first section

Give some background about what the trainees will be doing in the section.
Remember that many people reading your materials will likely be novices,
so make sure to explain all the relevant concepts.

## Title for a subsection
Section and subsection titles will be displayed in the tutorial index on the left side of
the page, so try to make them informative and concise!

# Hands-on Sections
Below are a series of hand-on boxes, one for each tool in your workflow file.
Often you may wish to combine several boxes into one or make other adjustments such
as breaking the tutorial into sections, we encourage you to make such changes as you
see fit, this is just a starting point :)

Anywhere you find the word "***TODO***", there is something that needs to be changed
depending on the specifics of your tutorial.

have fun!

## Get data

> ### {% icon hands_on %} Hands-on: Data upload
>
> 1. Create a new history for this tutorial
> 2. Import the files from [Zenodo]({{ page.zenodo_link }}) or from
>    the shared data library (`GTN - Material` -> `{{ page.topic_name }}`
>     -> `{{ page.title }}`):
>
>    ```
>    
>    ```
>    ***TODO***: *Add the files by the ones on Zenodo here (if not added)*
>
>    ***TODO***: *Remove the useless files (if added)*
>
>    {% snippet faqs/galaxy/datasets_import_via_link.md %}
>
>    {% snippet faqs/galaxy/datasets_import_from_data_library.md %}
>
> 3. Rename the datasets
> 4. Check that the datatype
>
>    {% snippet faqs/galaxy/datasets_change_datatype.md datatype="datatypes" %}
>
> 5. Add to each database a tag corresponding to ...
>
>    {% snippet faqs/galaxy/datasets_add_tag.md %}
>
{: .hands_on}

# Title of the section usually corresponding to a big step in the analysis

It comes first a description of the step: some background and some theory.
Some image can be added there to support the theory explanation:

![Alternative text](../../images/image_name "Legend of the image")

The idea is to keep the theory description before quite simple to focus more on the practical part.

***TODO***: *Consider adding a detail box to expand the theory*

> ### {% icon details %} More details about the theory
>
> But to describe more details, it is possible to use the detail boxes which are expandable
>
{: .details}

A big step can have several subsections or sub steps:


## Sub-step with **mofa**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. {% tool [mofa](mofa) %} with the following parameters:
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **compare_annotations**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. {% tool [compare_annotations](compare_annotations) %} with the following parameters:
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **cluster_mofa_embeddings_with_mudata**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. {% tool [cluster_mofa_embeddings_with_mudata](cluster_mofa_embeddings_with_mudata) %} with the following parameters:
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **rank_genes_and_peaks_muon**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. {% tool [rank_genes_and_peaks_muon](rank_genes_and_peaks_muon) %} with the following parameters:
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **assign_cell_type_labels_mudata**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. {% tool [assign_cell_type_labels_mudata](assign_cell_type_labels_mudata) %} with the following parameters:
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **visualise_markers_mudata**

> ### {% icon hands_on %} Hands-on: Task description
>
> 1. {% tool [visualise_markers_mudata](visualise_markers_mudata) %} with the following parameters:
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > ### {% icon comment %} Comment
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> ### {% icon question %} Questions
>
> 1. Question1?
> 2. Question2?
>
> > ### {% icon solution %} Solution
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}


## Re-arrange

To create the template, each step of the workflow had its own subsection.

***TODO***: *Re-arrange the generated subsections into sections or other subsections.
Consider merging some hands-on boxes to have a meaningful flow of the analyses*

# Conclusion
{:.no_toc}

Sum up the tutorial and the key takeaways here. We encourage adding an overview image of the
pipeline used.