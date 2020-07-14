# Trajectory and velocity analysis of scRNAseq COLON data 
## Part 0. Prerequisites
### Which packages are necessary to run this repository and how to setup them up in a coding environment

#### Introduction

The whole analysis in this repository is done in Python using just a few openly available packages. As long as you have the necessary libraries and data in correct format, you should be able to run this pipeline in whatever way you like. Below, I am showcasing how to set up a Jupyter Notebook running in a conda environment, since that's how it was done to get the results for the publication. 

#### Packages

In this guide I mostly make use of two (closely related) packages: [scanpy](https://scanpy.readthedocs.io/en/stable/) and [scVelo](https://scvelo.readthedocs.io/). Those two will install as dependencies many other packages, like numpy, pandas, or seaborn, some of which we will also use explicitly along the way. Apart from that, we will need two tools for doublet detection, [Scrublet](https://github.com/AllonKleinLab/scrublet) and [DoubletDetection](https://github.com/JonathanShor/DoubletDetection). 

#### Setting up the working environment

As mentioned above, my preferred way of working through this tutorial is with a use of [Jupyter Notebook](https://jupyter.org/). It is good practice to work in a separate environment, since installing new packages and dependencies can often break the previous ones. In which all you need to do is create a new environment. Much easier than reinstalling the system. A standard package manager is [conda](https://docs.conda.io/projects/conda/en/latest/index.html). 

There are distributions of conda: Miniconda and Anaconda. Miniconda is a command line tool, while Anaconda gives you a graphical interface. Here, I will show how to install and configure an environment in Miniconda on macOS. For other installation options please refer to the [official documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html). 

For installation of conda and other packages you need to be working within the command-line interface. On macOS you can use a build-in app **Terminal**.

Download the latest version of macOS installer and save to your home directory
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh -O ~/miniconda.sh
```

Run the installer in a *silent mode*: automatically accept default settings
```
bash ~/miniconda.sh -b -p $HOME/miniconda
```
Default should be good enough, but if you prefer to set things up manually, then run the above line without the `-b` and `-p` options.

The installer will prompt you: `Do you wish the installer to initialize Miniconda3 by running conda init?`. Enter `yes`.

With conda installed, we can begin configuring the environment we will need for work. 

Create the a new environment to work in (where `GitHubTest` is the name of your new environment)
```
conda create -n GitHubTest python=3.6
```

Activate (enter) the newly created environment with
```
conda activate GitHubTest
```

Install Scanpy's dependencies
```
conda install seaborn scikit-learn statsmodels numba pytables
conda install -c conda-forge python-igraph leiden
```
Install `scanpy`
```
pip install scanpy
```