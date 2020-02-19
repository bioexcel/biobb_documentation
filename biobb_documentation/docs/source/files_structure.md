# Files structure

[https://github.com/bioexcel/biobb_template](https://github.com/bioexcel/biobb_template)

Taking a look to the **biobb_template** files structure, we will find something like that:

* biobb_template/
    * \_\_init\_\_.py
    * adapters/
        * cwl/
        * pycompss/
    * docs/
    * json_schemas/
    * notebooks/
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

## biobb_template

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template](https://github.com/bioexcel/biobb_template/tree/master/biobb_template)

This folder contains the whole project.

### \_\_init\_\_.py

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/\_\_init\_\_.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/__init__.py)

Files named **\_\_init\_\_.py** are used to mark directories on disk as Python package directories. In this first level we define the package name (*biobb_template*) and all the modules contained in this package, only one in this example (*template*):


```python
name = "biobb_template"
__all__ = ["template"]
```

### adapters folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/adapters](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/adapters)

This folder contains templates' adapters to use the **BioBBs** with several workflow managers. More examples of **BioBB** adapters can be found at [https://github.com/bioexcel/biobb_adapters/](https://github.com/bioexcel/biobb_adapters/).

#### cwl folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/adapters/cwl](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/adapters/cwl)

This folder contains the [**Common Workflow Language**](https://www.commonwl.org/) template.

More information in the [CWL section](https://biobb-documentation.readthedocs.io/en/latest/adapters.html#cwl).

#### pycompss folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/adapters/pycompss](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/adapters/pycompss)

This folder contains the [**PyCOMPSs**](https://pypi.org/project/pycompss/) template.

More information in the [PyCOMPSs section](https://biobb-documentation.readthedocs.io/en/latest/adapters.html#pycompss).

### docs folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/docs](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/docs)

In this folder there are all the files related to the documentation of the project. Although most of the documentation is automatically generated through [**sphinx**](https://www.sphinx-doc.org/en/master/), the section indexes, readme and command line section must be made by hand.  

More information in the [Documentation section](https://biobb-documentation.readthedocs.io/en/latest/documentation.html).

### json_schemas folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/json_schemas](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/json_schemas)

The JSON Schemas of all the wrappers are stored in this tool. In addition, there is a JSON file with general information about the whole package.

More information in the [JSON Schemas section](https://biobb-documentation.readthedocs.io/en/latest/schemas.html#json-schemas).

### notebooks folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/notebooks](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/notebooks)

This folder contains a template with examples of the execution of *biobb_template* tools in [Jupyter Notebook](https://jupyter.org/).

### template folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/template](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/template)

In this folder, we can find all the wrappers. In the *biobb_template* case there are two: **template.py** and **template_container.py**

#### \_\_init\_\_.py

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/\_\_init\_\_.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/__init__.py)

Files named **\_\_init\_\_.py** are used to mark directories on disk as Python package directories. In this second level we define the folder name (*template*) and all the tools contained in this folder, only two in this example (*template* and *template_container*):


```python
name = "template"
__all__ = ["template", "template_container"]
```

#### template.py

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template.py)

Example with all the code needed to generate a common **BioExcel building block**. In this case, the tool to wrap is [zip](http://infozip.sourceforge.net/).

More information about the structure of this file can be found in the [Template section](https://biobb-documentation.readthedocs.io/en/latest/python_structure.html#template).

#### template_container.py

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/temp...](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template_container.py)

Example with all the code needed to generate a **BioExcel building block** executing the wrapped tool through a container. In this case, the tool to wrap is [zip](http://infozip.sourceforge.net/) and two containers are provided: [Docker Hub](https://hub.docker.com/r/mmbirb/zip) and [Singularity Hub](https://singularity-hub.org/) (type `singularity pull shub://bioexcel/zip_container` for installing it in your computer).

More information about the structure of this file can be found in the [TemplateContainer section](https://biobb-documentation.readthedocs.io/en/latest/python_structure.html#templatecontainer).

### test folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test)

In this folder there are all the needed files for the unittests: data files, reference files, python tests and configuration file.

More information about the files structure of this folder and the execution of tests can be found in the [Unittests secion](https://biobb-documentation.readthedocs.io/en/latest/execution.html#unittests).

## conda_env

[https://github.com/bioexcel/biobb_template/tree/master/conda_env](https://github.com/bioexcel/biobb_template/tree/master/conda_env)

This folder contains the two necessary files for building a new conda environment.

### biobb_template.pth

[https://github.com/bioexcel/biobb_template/blob/master/conda_env/biobb_template.pth](https://github.com/bioexcel/biobb_template/blob/master/conda_env/biobb_template.pth)

```Shell
/path/to/biobb_template/
/path/to/biobb_template/biobb_template
```

The two lines of this file must be edited and changed by the correct path to your *biobb_template* project. This paths file is used for import local python packages to our conda environment.

### environment.yml

[https://github.com/bioexcel/biobb_template/blob/master/conda_env/environment.yml](https://github.com/bioexcel/biobb_template/blob/master/conda_env/environment.yml)

```YAML
name: biobb_template
channels:
  - conda-forge
  - bioconda
dependencies:
  - python
  - biobb_common==2.0.1
  - nb_conda_kernels
  - nose
  - zip
  - conda
```

In this YAML file we find all the pakcages that will be installed in our conda environment:

* **python**: Last available version of python in anaconda.
* **biobb_common**: BioBB library with all the necessary functions for developing **BioExcel Building Blocks**.
* **nb_conda_kernels**: This extension enables a Jupyter Notebook application in one conda environment to access kernels for several languages languages found in other environments.
* **nose**: Unittest python library.
* **zip**: Software that will be wrapped in this *biobb_template* examples
* **conda**: Conda dependency.

## Root files

### .gitignore

[https://github.com/bioexcel/biobb_template/blob/master/.gitignore](https://github.com/bioexcel/biobb_template/blob/master/.gitignore)

A **.gitignore** file specifies intentionally untracked files that Git should ignore. 

### LICENSE

[https://github.com/bioexcel/biobb_template/blob/master/LICENSE](https://github.com/bioexcel/biobb_template/blob/master/LICENSE)

License for the package, usually **[Apache License 2.0](https://github.com/bioexcel/biobb_documentation/blob/master/LICENSE)** 

### README.md

[https://github.com/bioexcel/biobb_template/blob/master/README.md](https://github.com/bioexcel/biobb_template)

Text file containing useful information about the **BioBB** package.

### setup.py

[https://github.com/bioexcel/biobb_template/blob/master/setup.py](https://github.com/bioexcel/biobb_template/blob/master/setup.py)


```python
import setuptools

with open("README.md", "r") as fh:
    long_description = fh.read()

setuptools.setup(
    name="biobb_template",
    version="1.0.0",
    author="Biobb developers",
    author_email="your@email.com",
    description="Biobb_template is a complete code template to promote and facilitate the creation of new Biobbs by the community.",
    long_description=long_description,
    long_description_content_type="text/markdown",
    keywords="Bioinformatics Workflows BioExcel Compatibility",
    url="https://github.com/bioexcel/biobb_template",
    project_urls={
        "Documentation": "http://biobb_template.readthedocs.io/en/latest/",
        "Bioexcel": "https://bioexcel.eu/"
    },
    packages=setuptools.find_packages(exclude=['adapters', 'docs', 'json_schemas', 'notebooks', 'test']),
    install_requires=['biobb_common>=2.0.1'],
    python_requires='==3.6.*',
    classifiers=(
        "Development Status :: 3 - Alpha",
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: Apache Software License",
        "Operating System :: MacOS :: MacOS X",
        "Operating System :: POSIX",
    ),
)
```

In this **setup.py file**, the developers must fill several fields such as descriptions, author name, keywords and so on. The most important fields are:

* **version**: version of the package
* **packages**: the ones excluded won't be automatically parsed when building the documentation. For further information about this process, please visit the [Documentation section](https://biobb-documentation.readthedocs.io/en/latest/documentation.html).
* **install_requires**: dependencies of this package
* **python_requires**: version of python
