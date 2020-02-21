# Upload your package to bioconda

## Contact with us

First off, please send us an email to [bioexcel.biobb@gmail.com](mailto:bioexcel.biobb@gmail.com) explaining if you have developed a complete package or a new tool for an existing package. The processes to follow in both cases are slightly different. And in both cases we should give you some permissions.

## Download our script

Once we are in touch and you have all the permissions, you can download a **shell script** developed by our team that easies and automates all the process of generation of a new conda package:

[https://github.com/bioexcel/biobb/blob/master/new_conda_version.sh](https://github.com/bioexcel/biobb/blob/master/new_conda_version.sh)

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

After uploading to GitHub, please check that all the documentation generated in Read The Docs is correct:

[https://biobb-template.readthedocs.io/en/latest/index.html](https://biobb-template.readthedocs.io/en/latest/index.html)

More information about how to generate this documentation in the [Documentation section](https://biobb-documentation.readthedocs.io/en/latest/documentation.html).

### Create pypi repository

```Shell
python3 setup.py sdist bdist_wheel
python3 -m twine upload dist/* 
rm -rfv REPOSITORY.egg-info dist build
```

Note that you should change **REPOSITORY** by the name of your repository.

### Create Bioconda repository

```Shell
cd /home/user
conda skeleton pypi REPOSITORY
```

Note that you should change **REPOSITORY** by the name of your repository.

#### Create recipe

Open *REPOSITORY/meta.yaml* with your IDE and copy **version** and **sha256**.

#### Upload to bioconda

```Shell
cd /home/user/projects/bioconda-recipes/recipes
git checkout -f master
git pull origin master
git branch -D REPOSITORY
git push origin --delete REPOSITORY
git checkout -b REPOSITORY
```

Open folder */home/user/projects/bioconda-recipes/recipes/REPOSITORY* with your IDE.

Edit *build.sh* file:

```Shell
#!/usr/bin/env bash

python3 setup.py install --single-version-externally-managed --record=record.txt

mkdir -p $PREFIX/bin

chmod u+x $SP_DIR/biobb_analysis/gromacs/gmx_rms.py
cp $SP_DIR/biobb_analysis/gromacs/gmx_rms.py $PREFIX/bin/gmx_rms
```

TODO: Explain META.YML

TODO: Explain post-link.sh (NOT RECCOMENDED)

Note that you should change **REPOSITORY** by the name of your repository.

### Create Docker container

If you didn't use a *post-link.sh* file in the previous step, this process is automatic. Otherwise, you should write a Docker recipe and upload it to Docker Hub (TODO)

### Create Singularity container

TODO

#### Create recipe

#### Upload to Singularity
