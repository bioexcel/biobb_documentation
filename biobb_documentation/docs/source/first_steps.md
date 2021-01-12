# First steps

## Introduction

In the recent years, the improvement of **software** and **hardware performance** has made biomolecular simulations a mature tool for the study of biological processes. Simulation length and the size and **complexity** of the **analyzed systems** make simulations both complementary and compatible with other bioinformatics disciplines. However, the characteristics of the software packages used for simulation have prevented the adoption of the technologies accepted in other bioinformatics fields like automated **deployment systems**, **workflow orchestration**, or the use of **software containers**. We faced the challenge to bring biomolecular simulations to the “bioinformatics way of working”. The exercise has led to the development of the **BioExcel Building Blocks (BioBB's)** library. **BioBB's** are built as **Python wrappers** to provide an interoperable architecture. BioBB’s have been integrated in a chain of usual software management tools to generate **data ontologies**, **documentation**, **installation packages**, **software containers** and ways of integration with **workflow managers**, that make them usable in most computational environments.

### Useful links

The **BioExcel Building Blocks (BioBB's)** are described in the [official website](https://mmb.irbbarcelona.org/biobb/).

The repositories index of **BioExcel Building Blocks (BioBB's)** is available in [GitHub](https://github.com/bioexcel/biobb)

In this manual we will detail how to build a new **BioBB** module or package using the [biobb_template](https://github.com/bioexcel/biobb_template) as a base.

### Citation

When using **BioExcel Building Blocks (BioBB's)** please cite:

[BioExcel Building Blocks, a software library for interoperable biomolecular simulation workflows.](https://www.nature.com/articles/s41597-019-0177-4)<br>
Pau Andrio, Adam Hospital, Javier Conejero, Luis Jordá, Marc Del Pino, Laia Codo, Stian Soiland-Reyes, Carole Goble, Daniele Lezzi, Rosa M. Badia, Modesto Orozco & Josep Ll. Gelpi  *Nature Scientific Data*, 09/2019, Volume 6, Issue 1, p.169, (2019)

## New with anaconda?

**Anaconda** is a free and open-source distribution for scientific computing (data science, machine learning applications, large-scale data processing, predictive analytics, etc.), that aims to simplify package management and deployment. Package versions are managed by the package management system **conda**. The Anaconda distribution includes data-science packages suitable for **Windows**, **Linux**, and **MacOS**.

If you have never worked before with **Anaconda**, please take a look to the [official documentation](https://docs.anaconda.com/anaconda/install/) and install it in your computer.

In the tutorials section of our official website we have developed a series of **installation and execution** tutorials. As a part of these tutorials we explain how to install Anaconda in different operating systems:

* [Install Anaconda in **mac OS**](https://mmb.irbbarcelona.org/biobb/availability/tutorials/macos): steps for **installing Anaconda in mac OS** and once the environment is installed, execute one of our **BioBB tutorials**.
* [Install Anaconda in **Ubuntu**](https://mmb.irbbarcelona.org/biobb/availability/tutorials/ubuntu): steps for **installing Anaconda in Ubuntu** and once the environment is installed, execute one of our **BioBB tutorials**.
* [Install Anaconda in **Windows 10** using the **Windows Subsystem for Linux**](https://mmb.irbbarcelona.org/biobb/availability/tutorials/windows): as some of the dependencies of our packages are not fully compatible with Windows, here you will find how to enable **Windows Subsystem for Linux**, then install **Ubuntu** in **Windows 10** and finally **install Anaconda in Ubuntu**. Once the environment is installed, execute one of our **BioBB tutorials**.

## Installation

First off, please go to the *biobb_template* **GitHub repository** and install it in your computer as indicated in the **README.md** file in the repository:

[https://github.com/bioexcel/biobb_template](https://github.com/bioexcel/biobb_template)

### Download repository

Although the biobb_template repository is available at GitHub and thus you can clone it, we strongly recommend you to [**download it compressed**](https://github.com/bioexcel/biobb_template/archive/master.zip) and start your new project from scratch. 

### Create new conda environment

Once you have the project unzipped in your computer, please follow the next steps to create a new conda environment:

```Shell
cd biobb_template-master
conda env create -f conda_env/environment.yml
```

### Update environment paths

Edit *conda_env/biobb_template.pth* with the paths to your *biobb_template* folder. Example:

```Shell
/home/user_name/projects/biobb_template/
/home/user_name/projects/biobb_template/biobb_template/biobb_template
```

Copy the edited *conda_env/biobb_template.pth* file to the site-packages folder of your environment. This folder is in */[anaconda-path]/envs/biobb_template/lib/python3.7/site-packages*, where */[anaconda-path]* is usually */anaconda3* or */opt/conda*.

```Shell
cp conda_env/biobb_template.pth /[anaconda-path]/envs/biobb_template/lib/python3.7/site-packages
```

### Activate environment

Then, activate the recently created *biobb_template* conda environment:

```Shell
conda activate biobb_template
```

### Create repository

This template includes some folders not standard for a biobb, such as **biobb_template/adapters/**, **biobb_template/notebooks/** or **conda_env/**. For the sake of having a pure biobb structure, you should uncomment the three last lines of the **.gitignore** file before creating a new git repository:

```Shell
biobb_template/adapters
biobb_template/notebooks
conda_env
```

Then, inialitize repository:

```Shell
git init
```

### Binary paths configuration

Additionnally, it's recommendable to configure binary paths in your environment in order to ease the command line execution. More info about this subject can be found in the [Binary path configuration](https://biobb-documentation.readthedocs.io/en/latest/execution.html#binary-path-configuration) section.
