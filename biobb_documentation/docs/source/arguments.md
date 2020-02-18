# Arguments

All the **BioExcel building blocks** are wrappers of tools that accept **one or more** input file paths as **inputs**, **one or more** output file paths as **outputs** and, **optionally**, several **properties** to customize the execution of the wrapped tool.

## Inputs

It's mandatory to pass **at least one** input file path in **string** format. Inputs can be required or optional (but as said before, at least one must be required).

In the *biobb_template* example there are **two inputs**:

* **input_file_path1:** Defined as required. String with the path of the file.
* **input_file_path2:** Defined as optional. String with the path of the file.

A **config file** with the **properties** could also be considered as an input file, but as described below, the properties can be passed to a **BioBB** in several different ways.

## Outputs

It's mandatory to pass **at least one** output file path in **string** format. Outputs can be required or optional (but as said before, at least one must be required).

In the *biobb_template* example there is **one output**:

* **output_file_path:** Defined as required. String with the path of the file.

## Properties

Properties can be passed to a **BioBB** in different ways:

* As a file: creating a **config file** that will be passed to the Python class as an **input file path** in a command line call. Accepted formats:
    * **YAML**
    * **JSON**
 
```Shell
template --config template.yml --input_file_path1 input1 --input_file_path2 input2 --output_file_path output.zip
```
    
* As a string: in case we launch a **BioBB** through command line, we can also pass the properties in a string in **JSON format**.

```Shell
template --config '{"boolean_property":false}' --input_file_path1 input1 --input_file_path2 input2 --output_file_path output.zip
```

* As a Python dictionary: properties can also be passed to the Python class as a **Python dictionary** directly to the **properties argument** of the Python class.

```Python
prop = {
    "boolean_property": False
}
Template(input_file_path1=input_file_path1, 
         input_file_path2=input_file_path2, 
         output_file_path=output_file_path, 
         properties=prop).launch()
```

In the *biobb_template* example there are two wrappers:

* **template.py:** Wrapper for the zip tool.
* **template_container.py:** Wrapper for the zip tool executed through a container.

The properties for both examples are different since the execution of tools through container need specific properties.

### Template

Below there is the dictionary of properties for the **template.py** tool:

* **boolean_property** (*bool*) - (True) Example of boolean property.
* **executable_binary_property** (*str*) - ("zip") Example of executable binary property.
* **remove_tmp** (*bool*) - (True) [WF property] Remove temporal files.
* **restart** (*bool*) - (False) [WF property] Do not execute if output files exist.

There are two types of properties in this template:

#### Specific properties for this BioBB

* **boolean_property** (*bool*): is a property defined specifically for this **BioBB** as an example of specific property. There are several data types that can be used: **boolean** (*bool*), **integer** (*int*), **float** (*float*), **string** (*str*) and **dictionary** (*dic*).
* **executable_binary_property** (*str*): is used to define the path of the binary wrapped by the **BioBB**. Usually in case the user doesn't want to execute the default binary provided by the environment.

#### Properties common in all BioBB

* **can_write_console_log** (*bool*): Output log to console.
* **global_log** (*Logger object*): Log from the main workflow. Workflow property.
* **prefix** (*str*): Prefix if provided.
* **step** (*str*): Step indentifier in a workflow. Workflow property.
* **path** (*str*): Absolute path to the step working dir. Workflow property.
* **remove_tmp** (*bool*): Remove temporal files. Workflow property.
* **restart** (*bool*): Restart from previous execution. Workflow property.

As we can see, not all of these properties are defined in the **template.py** tool. If they are not defined, the system assigns default values to them.

### Template Container

Below there is the dictionary of properties for the **template_container.py** tool:

* **boolean_property** (*bool*) - (True) Example of boolean property.
* **executable_binary_property** (*str*) - ("zip") Example of executable binary property.
* **remove_tmp** (*bool*) - (True) [WF property] Remove temporal files.
* **restart** (*bool*) - (False) [WF property] Do not execute if output files exist.
* **container_path** (*str*) - (None) Container path definition.
* **container_image** (*str*) - ('mmbirb/zip:latest') Container image definition.
* **container_volume_path** (*str*) - ('/tmp') Container volume path definition.
* **container_working_dir** (*str*) - (None) Container working directory definition.
* **container_user_id** (*str*) - (None) Container user_id definition.
* **container_shell_path** (*str*) - ('/bin/bash') Path to default shell inside the container.

There are three types of properties in this template:

#### Specific properties for this BioBB

* **boolean_property** (*bool*): is a property defined specifically for this **BioBB** as an example of specific property. There are several data types that can be used: **boolean** (*bool*), **integer** (*int*), **float** (*float*), **string** (*str*) and **dictionary** (*dic*).
* **executable_binary_property** (*str*): is used to define the path of the binary wrapped by the **BioBB**. Usually in case the user doesn't want to execute the default binary provided by the environment.

#### Container specific properties

* **container_path** (*str*): Container path definition (docker / singularity).
* **container_image** (*str*): Container image definition (image indentifier for docker, path to image for singularity).
* **container_volume_path** (*str*): Container volume path definition. Path inside the container where the temporary files created by the wrapper will be mapped.
* **container_working_dir** (*str*): Container working directory definition. path inside the container where the job will be executed.
* **container_user_id** (*str*): Container user_id definition.
* **container_shell_path** (*str*): Path to default shell inside the container.

#### Properties common in all BioBB

* **can_write_console_log** (*bool*): Output log to console.
* **global_log** (*Logger object*): Log from the main workflow. Workflow property.
* **prefix** (*str*): Prefix if provided.
* **step** (*str*): Step indentifier in a workflow. Workflow property.
* **path** (*str*): Absolute path to the step working dir. Workflow property.
* **remove_tmp** (*bool*): Remove temporal files. Workflow property.
* **restart** (*bool*): Restart from previous execution. Workflow property.

As we can see, not all of these properties are defined in the **template_container.py** tool. If they are not defined, the system assigns default values to them.
