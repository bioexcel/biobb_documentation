# Upload your package to bioconda

## Contact with us

First off, please send us an email to [bioexcel.biobb@gmail.com](mailto:bioexcel.biobb@gmail.com) explaining if you have developed a complete package or a new tool for an existing package. The processes to follow in both cases are slightly different. And in both cases we should give you some permissions.

## Download our script

Once we are in touch and you have all the permissions, you can download a **shell script** developed by our team that easies and automates all the process of generation of a new conda package:

[https://github.com/bioexcel/utils_biobb/blob/master/new_version/new_conda_version.sh](https://github.com/bioexcel/utils_biobb/blob/master/new_version/new_conda_version.sh)

You just need to download it and change the variables by the values of your own computer:

```Shell
ide=subl
path_user=path_to_user_home
path_biobb=${path_user}path_to_biobb_project
path_json_schemas=path_to_json_generator/
```

## Steps

Then we will explain **all the steps** automatically done by the script defined in the previous section, plus the process to upload the package as a container to [Singularity hub](https://singularity-hub.org/).

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

##### build.sh file

In this file, we transform our Python files in binary files inside the bioconda container.

```shell
#!/usr/bin/env bash

python3 setup.py install --single-version-externally-managed --record=record.txt

mkdir -p $PREFIX/bin

chmod u+x $SP_DIR/REPOSITORY/MODULE/TOOL.py
cp $SP_DIR/REPOSITORY/MODULE/TOOL.py $PREFIX/bin/TOOL
```
In the *biobb_template* example:

* **REPOSITORY**: biobb_template
* **MODULE**: template
* **TOOL**: template / template_container

##### meta.yaml file

In this file we assign the build number and the requirements for the *biobb_template* package:

```yaml
{% set name = "biobb_template" %}
{% set version = "VERSION" %}
{% set file_ext = "tar.gz" %}
{% set hash_type = "sha256" %}
{% set hash_value = "HASH" %}

package:
  name: '\{\{ name|lower \}\}'
  version: '\{\{ version \}\}'

source:
  url: https://pypi.io/packages/source/{{ name[0] }}/{{ name }}/{{ name }}-{{ version }}.{{ file_ext }}
  '\{\{ hash_type \}\}': '\{\{ hash_value \}\}'

build:
  number: 0
  noarch: python

requirements:
  host:
    - python
    - setuptools
    - biobb_common ==3.5.1
    - zip
  run:
    - python
    - biobb_common ==3.5.1
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

### Create Docker container

> The BioBB development team strongly advise against use custom conda dependencies not included in the anaconda official channels (conda and conda-forge). In case of using only dependencies included in the anaconda official channels you can skip this section.

If you didn't use a *post-link.sh* file in the previous step, this process is automatic. Otherwise, you should write a Docker recipe and upload it to Docker Hub.

You can write a Docker recipe in thousands of different ways. Here you have an example:

```shell
FROM ubuntu:18.04

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial subversion

RUN wget --quiet https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc

RUN apt-get install -y curl grep sed dpkg && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v$\{TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean

RUN conda config --add channels defaults
RUN conda config --add channels bioconda
RUN conda config --add channels conda-forge
RUN conda install -y biobb_template==VERSION

ENTRYPOINT [ "/usr/bin/tini", "--" ]
CMD [ "/bin/bash" ]
```

Where **VERSION** is the version you want to install in this container.

### Create Singularity container

Once the Docker container has been created, we are ready for create the Singularity container.

#### Create recipe

If your Docker container has been created **automatically by Bioconda** and is in *quay.io*, the **Singularity recipe** *Singularity.latest* should be like this:

```shell
Bootstrap: docker
From: biobb_template:VERSION--py_0
Registry: quay.io
Namespace: biocontainers
```
Where **VERSION** is the version you want to install in this container.

If you have created the Docker container **by your own**, the **Singularity recipe** *Singularity.latest* should be like this:

```shell
Bootstrap: docker
From: biobb_template:VERSION
Namespace: REPOSITORY
```
Where **VERSION** is the version you want to install in this container and **REPOSITORY** is the name of your repository in [Docker Hub](https://hub.docker.com/).

#### Upload to Singularity

All the **BioBB** packages in **GitHub** are linked to **Singularity Hub** via [Webhooks](https://developer.github.com/webhooks/), so after **uploading to GitHub** the *Singularity.latest* file, you just must wait until the container is created in [Singularity Hub](https://singularity-hub.org/).
