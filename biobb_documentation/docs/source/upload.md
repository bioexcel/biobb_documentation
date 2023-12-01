# Upload your package to bioconda

## Contact with us

First off, please send us an email to [bioexcel.biobb@gmail.com](mailto:bioexcel.biobb@gmail.com) explaining if you have developed a complete package or a new tool for an existing package. The processes to follow in both cases are slightly different. And in both cases we should give you some permissions.

## Download our script

Once we are in touch and you have all the permissions, you can download a **shell script** developed by our team that easies and automates all the process of generation of a new conda package:

[https://github.com/bioexcel/utils_biobb/blob/master/utils_biobb/new_version/new_conda_version.sh](https://github.com/bioexcel/utils_biobb/blob/master/utils_biobb/new_version/new_conda_version.sh)

You just need to download it and change the variables by the values of your own computer:

```Shell
ide=subl
path_user=path_to_user_home
path_biobb=${path_user}path_to_biobb_project
path_json_schemas=path_to_json_generator/
```

## Steps

Then we will explain **all the steps** automatically done by the script defined in the previous section.

### Upload repository to GitHub

Upload data to [BioExcel GitHub](https://github.com/bioexcel):

```Shell
git status
git add . 
git commit -m "Message"
git push 
```

Tag with new version:

```Shell
git tag -a vVERSION -m "Message"
git push origin vVERSION
```
Note that tou should change **VERSION** by a correct version number.

### Chek Read The Docs documentation

All the **BioBB** packages in **GitHub** are linked to **Read the Docs** via [Webhooks](https://developer.github.com/webhooks/), so after **uploading to GitHub**, please check that all the documentation generated in **Read The Docs** is correct:

[https://biobb-template.readthedocs.io/en/latest/index.html](https://biobb-template.readthedocs.io/en/latest/index.html)

**More information** about how to generate this documentation in the [Documentation section](https://biobb-documentation.readthedocs.io/en/latest/documentation.html).

### Create pypi repository

```shell
python3 setup.py sdist bdist_wheel
python3 -m twine upload dist/* 
rm -rfv REPOSITORY.egg-info dist build
```

Note that you should change **REPOSITORY** by the name of your repository.

### Create Bioconda repository

```shell
cd /home/user
conda skeleton pypi REPOSITORY
```

Note that you should change **REPOSITORY** by the name of your repository.

#### Create recipe

The instruction above will create a new */home/user/REPOSITORY* folder. Then open */home/user/REPOSITORY/meta.yaml* with your IDE and copy **version** and **sha256**.

#### Upload to bioconda

```shell
cd /home/user/projects/bioconda-recipes/recipes
git checkout -f master
git pull origin master
git branch -D REPOSITORY
git push origin --delete REPOSITORY
git checkout -b REPOSITORY
```

Open folder */home/user/projects/bioconda-recipes/recipes/REPOSITORY* with your IDE.

##### meta.yaml file

In this file we assign the build number and the requirements for the *biobb_template* package:

```yaml
{% set name = "biobb_template" %}
{% set version = "VERSION" %}

package:
  name: '{{ name|lower }}'
  version: '{{ version }}'

source:
  url: https://pypi.io/packages/source/{{ name[0] }}/{{ name }}/{{ name }}-{{ version }}.tar.gz
  sha256: HASH

build:
  number: 0
  noarch: python
  script: "{{ PYTHON }} -m pip install . --no-deps --ignore-installed --no-cache-dir -vvv"
  run_exports:
    - {{ pin_subpackage(name, max_pin='x') }}

requirements:
  host:
    - python >=3.8
    - setuptools
    - biobb_common ==4.1.0
    - zip
  run:
    - python >=3.8
    - biobb_common ==4.1.0
    - zip
test:
  imports:
    - biobb_template
    - biobb_template.template

about:
  home: https://github.com/bioexcel/biobb_template
  license: Apache Software License
  license_family: APACHE
  license_file: ''
  summary: Biobb_template is a complete code template to promote and facilitate the creation of new Biobbs by the community.
  description: "Description for BioBB biobb_template"
  doc_url: ''
  dev_url: ''

extra:
  recipe-maintainers: ''
```

Where **VERSION** and **HASH** must be changed by the **version** and **sha256** values of the */home/user/REPOSITORY/meta.yaml* file created in the [Create recipe](#create-recipe) section.

##### post-link.sh file

> The BioBB development team strongly advise against use custom conda dependencies not included in the anaconda official channels (conda and conda-forge). In case of using only dependencies included in the anaconda official channels you can skip this section.

Although this file is not recommended because it can cause some issues in the conda installation, sometimes it's necessary for installing packages that are not in the official conda channels. This packages are installed at the end of the **Bioconda** package installation:

An example of *post-link.sh* code would be:

```shell
echo "Installing TOOL:"
conda install -y  -c CHANNEL TOOL==VERSION
```

### Containers

Note that Bioconda takes care of creating **automatically** the **Docker** and **Singularity** containers once the package is uploaded to [**anaconda.org**](https://anaconda.org/). Take into account that this process take a few hours. For finding the containers, please do:

* Docker: **https://quay.io/repository/biocontainers/<NAME_OF_PACKAGE>**

* Singularity: **https://depot.galaxyproject.org/singularity/<NAME_OF_PACKAGE>:<VERSION>--<BUILD>** where the build can be found in the Docker above. So, for example, in order to download the biobb_amber singularity, go to the following link: [**https://depot.galaxyproject.org/singularity/biobb_amber:4.1.0--pyhdfd78af_0**](https://depot.galaxyproject.org/singularity/biobb_amber:4.1.0--pyhdfd78af_0)

