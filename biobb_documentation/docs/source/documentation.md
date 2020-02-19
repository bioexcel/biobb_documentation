# Documentation

## Introduction

All the documentation of **BioExcel Building Blocks** is available in [Read the Docs](https://readthedocs.org/), a software documentation hosting platform where the source code is freely available, and the service is also free to use. It generates documentation that is written with the [Sphinx documentation generator](https://www.sphinx-doc.org/en/master/).

All the documentation of **BioBBs** is written in [**reStructuredText**](https://thomas-cokelaer.info/tutorials/sphinx/rest_syntax.html) and [**Markdown**](https://www.markdownguide.org/basic-syntax/):

* **reStructuredText** (RST, ReST, or reST) is a file format for textual data used primarily in the Python programming language community for technical documentation.
* **Markdown** is a lightweight markup language with plain-text-formatting syntax. Its design allows it to be converted to many output formats.

The documentation of a **BioBB** package is divided into three parts:

### Introduction and installation

[https://biobb-template.readthedocs.io/en/latest/readme.html](https://biobb-template.readthedocs.io/en/latest/readme.html)

**README.md** file written in **Markdown** language that is a copy of the main **README.md** file of the **BioBB** package. The **README.md** source code of de *biobb_template* documentation is located in:

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/docs/source/readme.md](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/docs/source/readme.md)

### API documentation

[https://biobb-template.readthedocs.io/en/latest/modules.html](https://biobb-template.readthedocs.io/en/latest/modules.html)

This section contains all the documentation related to the source code of the different modules of a **BioBB** package.

In order to **easy the process** of documentation, the **BioExcel Building Blocks** use the [sphinx autodoc extension](http://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html) that generates automatically all the documentation related to the source code respecting a certain **format for the comments** explained below in this same section.

### Command Line Documentation

[https://biobb-template.readthedocs.io/en/latest/command_line.html](https://biobb-template.readthedocs.io/en/latest/command_line.html)

This file is written in **Markdown**, though to easy the writting process (and also because there are some code executions in this process), we write it first in **Jupyter Notebook**, and then import it to **Markdown**.

* **Jupyter Notebook** file: [https://github.com/bioexcel/biobb_template/blob/master/biobb_template/docs/jupyter/command_line_template.ipynb](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/docs/jupyter/command_line_template.ipynb)

* **Markdown** file: [https://github.com/bioexcel/biobb_template/blob/master/biobb_template/docs/source/command_line.md](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/docs/source/command_line.md)

[Click here](https://biobb-template.readthedocs.io/en/latest/index.html) to access to the index page documentation of the *biobb_template*.

## Files structure

### \_\_init\_\_.py files

As explained in the [Files structure](https://biobb-documentation.readthedocs.io/en/latest/files_structure.html) section, there are two **\_\_init\_\_.py** files:

* **biobb_template/biobb_template/\_\_init\_\_.py**: define the package name (*biobb_template*) and all the modules contained in this package, only one in this example (*template*).


```python
name = "biobb_template"
__all__ = ["template"]
```

* **biobb_template/biobb_template/template/\_\_init\_\_.py**: define the folder name (*template*) and all the tools contained in this folder, only two in this example (*template* and *template_container*).


```python
name = "template"
__all__ = ["template", "template_container"]
```

This two files are used by the [sphinx autodoc extension](http://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html) for parsing the directories structure of our **BioBB**. Therefore, it's very important to define them properly and put only the **modules / tools** we want to documentate automatically.

### docs folder

The structure of the docs folder is as follows:

* docs/
    * jupyter/
        * command_line_template.ipynb
    * source/
        * \_static/
        * command_line.md
        * conf.py
        * index.rst
        * modules.rst
        * readme.md
        * schema.html
        * template.rst
    * Makefile
    


## Formats in code comments

### Arguments

### Functions

## Jupyter Notebook

some text
