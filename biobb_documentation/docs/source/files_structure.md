# Files structure

[https://github.com/bioexcel/biobb_template](https://github.com/bioexcel/biobb_template)

Taking a look to the **biobb_template** files structure, we will find something like that:

* .github
    * workflows
        * linting_and_testing.yml
    * env.yaml
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
* .readthedocs.yaml
* LICENSE
* README.md
* setup.py

## .github folder

[https://github.com/bioexcel/biobb_template/tree/master/.github](https://github.com/bioexcel/biobb_template/tree/master/.github)

This folder contains all the files necessary for **testing** the biobb package in the GitHub repository through [**GitHub Actions**](https://github.com/features/actions).

### env.yaml

[https://github.com/bioexcel/biobb_template/blob/master/.github/env.yaml](https://github.com/bioexcel/biobb_template/blob/master/.github/env.yaml)

File with all the dependencies needed for running the biobb package in a conda environment.

### workflows folder

[https://github.com/bioexcel/biobb_template/tree/master/.github/workflows](https://github.com/bioexcel/biobb_template/tree/master/.github/workflows)

This folder contains the **linting_and_testing.yml** file that allows to run the tests.

#### linting_and_testing.yml

[https://github.com/bioexcel/biobb_template/blob/master/.github/workflows/linting_and_testing.yml](https://github.com/bioexcel/biobb_template/blob/master/.github/workflows/linting_and_testing.yml)

This file is the workflow that runs the tests in an external Virtual Machine configured to run automatically through GitHub actions. When the developer pushes code to the repository, the workflow is launched performing linting and running the tests developed in the [Unittests](https://biobb-documentation.readthedocs.io/en/latest/unittests.html#unittests) section.

The structure of this file is explained in the [Github Actions](https://biobb-documentation.readthedocs.io/en/latest/unittests.html#github-actions) section.

## biobb_template

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template](https://github.com/bioexcel/biobb_template/tree/master/biobb_template)

This folder contains the whole project.

### \_\_init\_\_.py

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/\_\_init\_\_.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/__init__.py)

Files named **\_\_init\_\_.py** are used to mark directories on disk as Python package directories. In this first level we define the package name (*biobb_template*) all the modules contained in this package -only one in this example (*template*)- and the package version:


```python
name = "biobb_template"
__all__ = ["template"]
__version__ = "3.9.0"
```

### adapters folder

> Take into account that this folder is not standard for a biobb. Before add your biobb to a new Git repository it's strongly recommended to add it to the **.gitignore** file as explained in [this section](#gitignore)

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

> Take into account that this folder is not standard for a biobb. Before add your biobb to a new Git repository it's strongly recommended to add it to the **.gitignore** file as explained in [this section](#gitignore)

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

More information about the structure of this file can be found in the [Template section](https://biobb-documentation.readthedocs.io/en/latest/python_structure.html#template-class).

#### template_container.py

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template_container.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template_container.py)

Example with all the code needed to generate a **BioExcel building block** executing the wrapped tool through a container. In this case, the tool to wrap is [zip](http://infozip.sourceforge.net/) and two containers are provided: [Docker Hub](https://hub.docker.com/r/mmbirb/zip) and [Singularity Hub](https://singularity-hub.org/) (type `singularity pull --name zip.sif shub://bioexcel/zip_container` for installing it in your computer in *.sif* format or `singularity pull shub://bioexcel/zip_container` for installing it in your computer in *.simg* format).

More information about the structure of this file can be found in the [TemplateContainer section](https://biobb-documentation.readthedocs.io/en/latest/python_structure.html#templatecontainer-class).

### test folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test)

In this folder there are all the needed files for the unittests: data files, reference files, python tests and configuration file.

More information about the files structure of this folder and the execution of tests can be found in the [Unittests secion](https://biobb-documentation.readthedocs.io/en/latest/unittests.html).

## conda_env

> Take into account that this folder is not standard for a biobb. Before add your biobb to a new Git repository it's strongly recommended to add it to the **.gitignore** file as explained in [this section](#gitignore)

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
  - biobb_common>=3.9.0
  - nb_conda_kernels
  - pytest
  - zip
  - conda
```

In this YAML file we find all the pakcages that will be installed in our conda environment:

* **python**: Last available version of python in anaconda.
* **biobb_common**: BioBB library with all the necessary common functions for developing **BioExcel Building Blocks**.
* **nb_conda_kernels**: This extension enables a Jupyter Notebook application in one conda environment to access kernels for several languages found in other environments.
* **pytest**: Unittest python library.
* **zip**: Software that will be wrapped in this *biobb_template* examples
* **conda**: Installs conda client for easy the different conda instructions execution.

## Root files

### .gitignore

[https://github.com/bioexcel/biobb_template/blob/master/.gitignore](https://github.com/bioexcel/biobb_template/blob/master/.gitignore)

A **.gitignore** file specifies intentionally untracked files that Git should ignore. 

The biobb_template includes some folders not standard for a biobb, such as **biobb_template/adapters/**, **biobb_template/notebooks/** or **conda_env/**. For the sake of having a pure biobb structure, you should uncomment the three last lines of the .gitignore file before creating a new git repository:

```console
biobb_template/adapters
biobb_template/notebooks
conda_env
```

### .readthedocs.yaml

[https://github.com/bioexcel/biobb_template/blob/master/.readthedocs.yaml](https://github.com/bioexcel/biobb_template/blob/master/.readthedocs.yaml)

A **.readthedocs.yaml** file configures your documentation builds with [Read the Docs](https://readthedocs.org/)

### LICENSE

[https://github.com/bioexcel/biobb_template/blob/master/LICENSE](https://github.com/bioexcel/biobb_template/blob/master/LICENSE)

License for the package, usually **[Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)** 

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
    version="3.9.0",
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
    packages=setuptools.find_packages(exclude=['adapters', 'docs', 'test']),
    install_requires=['biobb_common>=3.9.0'],
    python_requires='>=3.7,<3.10',
    classifiers=(
        "Development Status :: 3 - Alpha",
        "Programming Language :: Python :: 3.7",
        "License :: OSI Approved :: Apache Software License",
        "Operating System :: MacOS :: MacOS X",
        "Operating System :: POSIX",
    ),
)
```

In this **setup.py file**, the developers must fill several fields such as descriptions, author name, keywords and so on. The most important fields are:

* **version**: version of the package
* **packages**: the ones excluded won't be automatically parsed in the building process of bioconda. It's important to exclude all the folders containing Python files that must be excluded in this building process.
* **install_requires**: dependencies of this package
* **python_requires**: version of python
