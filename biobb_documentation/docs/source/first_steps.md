# First steps

## Introduction

In the recent years, the improvement of **software** and **hardware performance** has made biomolecular simulations a mature tool for the study of biological processes. Simulation length and the size and **complexity** of the **analyzed systems** make simulations both complementary and compatible with other bioinformatics disciplines. However, the characteristics of the software packages used for simulation have prevented the adoption of the technologies accepted in other bioinformatics fields like automated **deployment systems**, **workflow orchestration**, or the use of **software containers**. We faced the challenge to bring biomolecular simulations to the “bioinformatics way of working”. The exercise has led to the development of the **BioExcel Building Blocks (BioBB)** library. BioBB’s are built as **Python wrappers** to provide an interoperable architecture. BioBB’s have been integrated in a chain of usual software management tools to generate **data ontologies**, **documentation**, **installation packages**, **software containers** and ways of integration with **workflow managers**, that make them usable in most computational environments.

### Useful links

The **BioExcel Building Blocks** are described in the [official website](http://mmb.irbbarcelona.org/biobb/).

The repositories index of **BioExcel Building Blocks** is available in [GitHub](https://github.com/bioexcel/biobb)

In this manual we will detail how to build a new BioBB module or package using the [biobb_template](https://github.com/bioexcel/biobb_template) as a base.

### Citation

When using **BioExcel Building Blocks** please cite:

[BioExcel Building Blocks, a software library for interoperable biomolecular simulation workflows.](https://www.nature.com/articles/s41597-019-0177-4)<br>
Pau Andrio, Adam Hospital, Javier Conejero, Luis Jordá, Marc Del Pino, Laia Codo, Stian Soiland-Reyes, Carole Goble, Daniele Lezzi, Rosa M. Badia, Modesto Orozco & Josep Ll. Gelpi  *Nature Scientific Data*, 09/2019, Volume 6, Issue 1, p.169, (2019)

## New with anaconda?

**Anaconda** is a free and open-source distribution for scientific computing (data science, machine learning applications, large-scale data processing, predictive analytics, etc.), that aims to simplify package management and deployment. Package versions are managed by the package management system **conda**. The Anaconda distribution includes data-science packages suitable for **Windows**, **Linux**, and **MacOS**.

If you have never worked before with **Anaconda**, please take a look to the [official documentation](https://docs.anaconda.com/anaconda/install/) and install it in your computer.

## Installation

First off, please go to the biobb_template GutHub repository and install it in your computer following the steps indicated in the README.md file in the repository:

https://github.com/bioexcel/biobb_template

### Clone repository

```Shell
git https://github.com/bioexcel/biobb_template.git
```

### Create new conda environment

```Shell
cd biobb_template
conda env create -f conda_env/environment.yml
```

### Update environment paths

Edit *conda_env/biobb_template.pth* with the paths to your *biobb_template* folder. Example:

```Shell
/home/user_name/projects/biobb_template/
/home/user_name/projects/biobb_template/biobb_template/biobb_template
```

Copy the *conda_env/biobb_template.pth* file to the site-packages folder of your environment. This folder is in */[anaconda-path]/envs/biobb_template/lib/python3.6/site-packages*, where */[anaconda-path]* is usually */anaconda3* or */opt/anaconda*.

```Shell
cp conda_env/biobb_template.pth </anaconda-path/envs/biobb_template/lib/python3.6/site-packages>
```

### Activate environment

```Shell
conda activate biobb_template
```

## Files structure

Taking a look to the biobb_template files structure, we will find something like that:

* biobb_template/
	* biobb_template/
		* \_\_init\_\_.py
        * cwl/
		* docs/
		* json_schemas/
        * notebooks/
        * pycompss/
        * template/
			* \_\_init\_\_.py
			* template.py
			* template_container.py
		* test/
    * conda_env/
        * biobb_template.pth
        * environment.yml
	* .gitignore
	* LICENSE
	* README.md
	* setup.py
