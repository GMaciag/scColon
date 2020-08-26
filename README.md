# Trajectory and velocity analysis of scRNAseq data 
## Guide for scRNAseq trajectory and velocity analysis complementary to the study "Widespread epithelial dedifferentiation in patients with ulcerative colitis"

### Introduction

This is part of a series of notebooks guiding you through of a full process of single cell data analysis. Starting with raw sequencing files we end up with differentiation trajectories and velocity plots. Along the way, we pay special attention to all the parameter choices and descriptions of results, so hopefully one can easily follow and replicate this analysis on their own data. 

At the same time, those notebooks present, in full extent, the computations done for the study **"Widespread epithelial dedifferentiation in patients with ulcerative colitis"**, published in *[manuscript in review]*. The data used for the notebooks come from that study and can be freely accessed online at *[in preparation]*.    

### Content description

This repository is divided into parts, each in its own folder. Inside, there are notebooks for each of the datasets analysed:
- Healthy
- Healthy progenitors
- Non-ulcerated
- Ulcerated
- All (Healthy, non- and ulcerated combined).

Notebooks for the healthy dataset are the default ones and that's where I include the most descriptions and deliberations. The vast majority of analysis for the other datasets is analogous and so I spare myself writing the same things multiple times. I'm showing the code used for all the datasets for completion sake. Please always refer to the healthy dataset notebooks for a more detailed description of each step. 

This guide assumes that you have data from multiple samples which you want to group together into conditions (for example *treated* and *untreated*). If you are working only on a single sample (or prefer to analyse everything separately), then there are some steps you will have to omit (especially in **Part 2**). 

### Content list

**Part 0** describes what is first required, in terms of data and software, in order to run the rest of the tutorial. Here I show how you need to preprocess your raw sequencing data so that you can get nice RNA velocity plots in the end. I also explain how you should set up programming environment on your computer and which packages will you need.  

In **Part 1** we first load the data from the `.loom` files and then go through a throughout quality control of the datasets. We filter the cells as well as the genes and concatenate the samples to be ready for the next step.

**Part 2** covers the normalization, regression and batch correction, which are all necessary to align the samples together and make them comparable. In this step we also calculate the proliferation and cell cycle scores. The batch correction method included in this part takes VERY long time and can easily run for hours on a bigger dataset. Beware and plan running your pipeline accordingly.

**Part 3** is where we perform dimensionality reduction, including PCA and UMAP. PCA let's us inspect where most of the variation Here we also have the opportunity to make first diagnostic plots, checking the quality of sample alignment and distribution of know marker genes. 

In **Part 4** we use the results of differential expression and our knowledge of known marker genes to divide the cells into informative clusters. We also explore differentiation trajectories.

**Part 5** is all about RNA velocity. After VERY, VERY long computation we are able to present the velocity on previously created embeddings (PCA, UMAP). Calculation of velocity gives us also access to the measurement of latent time, an unbiased alternative to pseudo time. 

**(((TBD)))** Finally, **Part 6** contains all other code snippets that were used in the original study, but that did not fit into any other part of the guide. Here for example I show how to subset a dataset to only one cell type and reanalyze it (shown on the example of Tuft and EECs cells from the study). This part may be updated in the future if more ideas on how to evaluate the dataset come about.

**NB!** This guide by no means exhausts the topic. You may have many questions and issues which I have not touched upon or did not explain well enough. It will be beneficial for your understanding if you look for additional sources of information about this type of analysis. I can highly recommend the [best practices in scRNAseq tutorial](https://www.embopress.org/doi/10.15252/msb.20188746) by Fabian Theis.  

## TO DO
- [ ] Finish Part 5, RNA velocity, for all conditions
- [ ] Finish Part 6, extra code, for all conditions
- [ ] Add hyperlinks to parts description in README
- [ ] Proofread the whole thing
- [ ] Turn into github.io page

## DONE
- [x] Finish readme, introduction to the guide and organization of code
- [x] Finish Part 0, prerequisites to running the guide
- [x] Finish Part 1, QC, for all conditions
- [x] Finish Part 2, normalisation, for all conditions
- [x] Finish Part 3, dimensionality reduction, for all conditions
- [x] Finish Part 4, celltype annotation, for all conditions