# Templates

In this manual we will detail how to build a new **BioBB** module or package using the [biobb_template](https://github.com/bioexcel/biobb_template) as a base. In this template, you will find all the material needed for the implementation of a new **BioBB**. The complete file structure is described in detail in the [Files structure](https://biobb-documentation.readthedocs.io/en/latest/files_structure.html) section.

In the *biobb_template* we provide, amongst the rest of materials, two Python templates: **template.py**, for wrapping a tool, and **template_container.py**, for wrapping a tool executed inside a container.

## Template (template.py)

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template.py)

This is the basic template for wrapping a tool. This Python file is a wrapper for the [**zip**](http://infozip.sourceforge.net/) tool. The process followed by this script is the following one:

* Receive **input(s)** paths. More information about the inputs can be found in the [Inputs section](https://biobb-documentation.readthedocs.io/en/latest/arguments.html#inputs).
* Receive **output** path. More information about the outputs can be found in the [Outputs section](https://biobb-documentation.readthedocs.io/en/latest/arguments.html#outputs).
* Receive **properties**. More information about the properties can be found in the [Properties section](https://biobb-documentation.readthedocs.io/en/latest/arguments.html#properties). And more information about the specific properties of this template can be found in the [Properties Template](https://biobb-documentation.readthedocs.io/en/latest/arguments.html#template) section.
* Create a **temporary folder** and copy the **input(s) file(s)** inside it. 
* Wrap the [**zip**](http://infozip.sourceforge.net/) tool creating a command line from the input(s) and properties.
* Execute the command line.
* Remove the **temporary folder** if the user has not said otherwise.
* If everything worked, the output file will be in the path provided to the tool.

More information about the structure of this file can be found in the [Template section](https://biobb-documentation.readthedocs.io/en/latest/python_structure.html#template).

## Template Container (template_container.py)

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template_container.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template_container.py)

This is the template for wrapping a tool executed through a container. In this case, the tool to wrap is [**zip**](http://infozip.sourceforge.net/) and two containers are provided: [Docker Hub](https://hub.docker.com/r/mmbirb/zip) and [Singularity Hub](https://singularity-hub.org/) (type `singularity pull --name zip.sif shub://bioexcel/zip_container` for installing it in your computer). The process followed by this script is the following one:

* Receive **input(s)** paths. More information about the inputs can be found in the [Inputs section](https://biobb-documentation.readthedocs.io/en/latest/arguments.html#inputs).
* Receive **output** path. More information about the outputs can be found in the [Outputs section](https://biobb-documentation.readthedocs.io/en/latest/arguments.html#outputs).
* Receive **properties**. More information about the properties can be found in the [Properties section](https://biobb-documentation.readthedocs.io/en/latest/arguments.html#properties). And more information about the specific properties of this template can be found in the [Properties Template Container](https://biobb-documentation.readthedocs.io/en/latest/arguments.html#template-container) section.
* Creation a **temporary folder** and map it to the *container_volume_path* path.
* Wrap the [**zip**](http://infozip.sourceforge.net/) tool creating a command line from the input(s) and properties.
* Execute the command line.
* Copy output file(s) from the mapped *container_volume_path* inside the container to the definitive output path defined by the user in the arguments.
* Remove the **temporary folder** if the user has not said otherwise.
* If everything worked, the output file will be in the path provided to the tool.

More information about the structure of this file can be found in the [Template Container section](https://biobb-documentation.readthedocs.io/en/latest/python_structure.html#template-container).
