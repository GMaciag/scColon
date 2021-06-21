# Trajectory and velocity analysis of scRNAseq COLON data 
## Part 0. Prerequisites
### Which packages are necessary to run this repository and how to setup them up in a programming environment

#### Introduction

The whole analysis in this repository is done in Python using just a few openly available packages. As long as you have the necessary libraries and data in a correct format, you should be able to run this pipeline in whatever way you like. The section **Setting up the working environment** is for people who need to start from scratch, but don't have much experience with programming. Below, I am showcasing how to set up a Jupyter Notebook running in a conda environment, since that's how it was done to get the results for the publication. 

#### Packages

In this guide I mostly make use of two (closely related) packages: [scanpy](https://scanpy.readthedocs.io/en/stable/) and [scVelo](https://scvelo.readthedocs.io/). Those two will install as dependencies many other packages, like numpy, pandas, or seaborn, some of which we will also use explicitly along the way. We will also need [mnnpy](https://github.com/chriscainx/mnnpy) and [bbknn](https://github.com/Teichlab/bbknn) for batch correction, which we need to install separately. Apart from that, we will need two tools for doublet detection, [Scrublet](https://github.com/AllonKleinLab/scrublet) and [DoubletDetection](https://github.com/JonathanShor/DoubletDetection). 

#### Setting up the working environment

As mentioned above, my preferred way of working through this tutorial is with a use of [Jupyter Notebook](https://jupyter.org/) while working in a conda environment. It is good practice to work in a separate environment, since installing new packages and dependencies can often break the previous ones. A standard package manager is [conda](https://docs.conda.io/projects/conda/en/latest/index.html). 

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
Default should be good enough, but if you prefer to set things up manually, then run the above line without the `-b` and `-p` options. The installer will prompt you: `Do you wish the installer to initialize Miniconda3 by running conda init?`. Enter `yes`.

With conda installed, we can begin configuring the environment we will need for work. 

Create a new environment to work in (where `GitHubTest` is the name of your new environment)
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
conda install -c conda-forge python-igraph leidenalg
```
Install `scanpy`
```
pip3 install scanpy
```
Install `scVelo`
```
pip3 install -U scvelo
```
Install `DoubletDetection`
```
git clone https://github.com/JonathanShor/DoubletDetection.git
cd DoubletDetection
pip3 install .
```
Install `scrublet`
```
pip3 install scrublet
```
Depending on your on OS and configuration this step may fail. It does for me, running on macOS Mojave. The issue for me was installation of the `annoy` package which failed because of incompatible C++ compiler. In this case run first
```
conda install -c akode annoy
```
before running the `scrublet` command. It should work now. 

Install `mnnpy` for batch correction with the MNNs algorithm
```
pip3 install mnnpy
```
Again, on macOS this step may fail. If the error is something like: `unable to execute '/usr/local/Cellar/gcc/9.2.0_1/bin/gcc-9': No such file or directory`, then the solution is to run first
```
export CC=/usr/local/Cellar/gcc/X.X.X/bin/gcc-X
```
where you should replace the 'X's with the version number you find on your machine. 

Install `bbknn` for batch correction with the bbknn algorithm
```
pip3 install bbknn
```

Now we install `Jupyter Notebook`. It's a web-based interactive development environment .
```
conda install -c conda-forge notebook
```
Before we begin, it's better to create a new directory in which you will run your jupyter notebook (and as such, the whole pipeline). That way you can keep all your files (both data and plots) organized. 
```
mkdir ~/GitHubTest
cd ~/GitHubTest/
```
Finally, the last thing left to do is actually run the notebook and start coding!
```
jupyter notebook
```
Jupyter Notebook should automatically open up in your default browser. If it does not (or you close the page), look through the output in the terminal for lines
```
[I 11:41:26.211 NotebookApp] The Jupyter Notebook is running at:
[I 11:41:26.211 NotebookApp] http://localhost:8888/
```
In this case it means you have to navigate to the the address `http://localhost:8888/` in your web browser. 

 **NB.** When you open a jupyter notebook for the first time, it will ask you to set a password. You will later need that password to login in the browser. Better note it down, but if you ever forget it, you can set a new one with
 ```
jupyter notebook password
 ```

 **NB. II** Remember that whenever you will want to run the pipeline, you need to start the Notebook within the conda environment we created before. You can know where you are by checking the command line prompt:
 ```
 (GitHubTest) MacBook-Pro:~ user$
 ```
 Text in brackets `( )`  should be the name of the environment you created. If it says `(base)`, you need to first run `conda activate GitHubTest` and just then `jupyter notebook`.
