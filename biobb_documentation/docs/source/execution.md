# Execution

## Execution in command line

### Binary path configuration

First off, we recommend to configure binary paths in your environment in order to easy the command line execution. For doing that, please follow the next steps:

#### Create binary folder

Remember to change the the */home/user_name/projects/* path to the real path on your computer.

```Shell
mkdir /home/user_name/projects/bin
```

#### Change python file permissions

Remember to change the the */home/user_name/projects/* path to the real path on your computer.

```Shell
chmod +x /home/user_name/projects/biobb_template/biobb_template/template/template.py
```

#### Create symlink to python file

Remember to change the the */home/user_name/projects/* path to the real path on your computer.

```Shell
cd /home/user_name/projects/bin
ln -s /home/user_name/projects/biobb_template/biobb_template/template/template.py template
```

#### Create conda activate actions

Remember to change the */[anaconda-path]* and the */home/user_name/projects/* path to the real paths on your computer.

```Shell
cd /[anaconda-path]/envs/biobb_template/etc
mkdir conda
cd conda/
mkdir activate.d
cd activate.d/
printf '#!/usr/bin/env bash\n\nexport BIOBB_OLD_PATH=$PATH\nexport PATH=/home/user_name/projects/bin:$PATH' > biobb_template.sh
```

#### Create conda deactivate actions

Remember to change the */[anaconda-path]* and the */home/user_name/projects/* path to the real paths on your computer.

```Shell
cd /[anaconda-path]/envs/biobb_template/etc/conda
mkdir deactivate.d
cd deactivate.d/
printf '#!/usr/bin/env bash\n\nexport PATH=$BIOBB_OLD_PATH' > biobb_template.sh
```

#### Restart environment

```Shell
conda deactivate
conda activate biobb_template
```

## Execution in Jupyter Notebook

some text

## Unittests

some text
