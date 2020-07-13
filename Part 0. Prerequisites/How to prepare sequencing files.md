# Trajectory and velocity analysis of scRNAseq COLON data
## Part 0. Prerequisites
### How to prepare sequencing files for further processing and analysis

#### Introduction

Though this series of notebooks is primarily concerned with downstream analysis of single cell data (post expression matrix creation), here I will give some guidelines on how you should format and preprocess your data, so that it is ready to be used in the following steps. Please note that this is not the only way to do it and you can most likely follow my tutorial, even if, for instance, your data was generated with a different technology. In the end, what is most important, is that you have the correct input data needed.

#### Formatting of input files

In most scRNAseq pipelines all you need to start with is an expression matrix, which can be prepared in many different ways and supplied in any format (even just in `.csv` or `.txt`). Since in this tutorial we want to calculate in the end the RNA velocity, that severely limits our options.

In general, you want to have a `.loom` file ( http://loompy.org/ ) with, at least, three layers - expression matrix (raw counts of reads), spliced reads and unspliced reads. The latter two are used for calculating velocity.

#### Processing of raw scRNA-seq reads

In the original study the single cell libraries were sequenced with the 10x Genomics Chromium platform and here I am assuming your data was generated in the same way.

It is the most straightforward to process 10x Genomics data with their own software, [Cell Ranger](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/what-is-cell-ranger). The software uses quite a lot of computational resources, so your best bet is to run it on a server. 

The `cellranger` workflow starts by demultiplexing the sequencing base call files (BCLs) into FASTQ files. If you already have FASTQ files, please skip to the next part. BCL files are demultiplexed with the `cellranger mkfastq` command. This step is a little bit more involved, but can be easily followed with the official [tutorial](https://support.10xgenomics.com/single-cell-vdj/software/pipelines/latest/using/mkfastq). 

The next step is to align the reads to a reference genome and then count them (how many reads per gene). This is the simplest done with the `cellranger count` [function](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/tutorial_ct). Now, theoretically it is not necessary to count the reads at this point, since we will do it again in a moment with the software for RNA velocity. However, I found this commands to be the easiest to use. Additionally, the `cellranger` pipeline will provide you with an alternative results (which you can check with, for example, [loupe browser](https://support.10xgenomics.com/single-cell-gene-expression/software/visualization/latest/what-is-loupe-cell-browser)) very useful for quick sanity checks on your data (read quality, marker gene expression), before the rest of the pipeline is ready.

Cell Ranger outputs A LOT of data, but for the next step you'll need only the sorted bam file. It can be found in the `your_sample_name/outs` folder under the name `possorted_genome_bam.bam`. 

#### Quantifying un/spliced reads

The last step is to quantify the mapped reads. For this we will use the [velocyto software](http://velocyto.org/). Velocyto has a few different implementations, but here we will be using the [command line tool](http://velocyto.org/velocyto.py/tutorial/index.html#running-the-cli). Again, you need a powerful computer to run it, so it is best done on a server. 

Velocyto counts reads falling into exonic/intronic regions and generates spliced/unspliced expression matrices in a loom file (apart from a regular expression matrix). Velocyto includes a shortcut to run the counting directly Cell Ranger output with the `velocyto run10x` [command](http://velocyto.org/velocyto.py/tutorial/cli.html#run10x-run-on-10x-chromium-samples). You need to point the software to your sample folder, which is the folder **containing** the subfolder `outs`.

The last thing you need is the `.gtf` genome annotation file. It is provided with the Cell Ranger pipeline and can be downloaded [here](https://support.10xgenomics.com/single-cell-gene-expression/software/downloads/latest).

If all goes well, `velocyto` will make the `.loom` file and put in a new folder called `velocyto` placed in your sample folder. If you were working on a server, copy that `.loom` file to a local directory, since we will need it for the next step, described in the **Part 1** of this tutorial. 