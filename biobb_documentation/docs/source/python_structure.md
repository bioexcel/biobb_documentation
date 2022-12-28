# Python structure

In this section we will describe the basic structure of the wrapped tool python files.

## Template class

Structure description for the **Template** class of the *biobb_template/biobb_template/template/template.py* file. The complete source code is available in the [biobb_template repository](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template.py).

### Imports


```python
import argparse
import shutil
from pathlib import PurePath
from biobb_common.generic.biobb_object import BiobbObject
from biobb_common.configuration import  settings
from biobb_common.tools import file_utils as fu
from biobb_common.tools.file_utils import launchlogger
```

Libraries used for the execution of the **Template** class.

### Class
Class template inherits from BiobbObject parent class.


```python
# 1. Rename class as required
class Template(BiobbObject):
```

### Docstrings

The automatic documentation generation system through [**sphinx**](https://www.sphinx-doc.org/en/master/) is explained in detail in the [Documentation section](https://biobb-documentation.readthedocs.io/en/latest/documentation.html#formats-in-code-comments).

After the definition of the Template class we define all the arguments explained in the [Arguments](https://biobb-documentation.readthedocs.io/en/latest/arguments.html#arguments) section inside a set of triple quotes:


```python
"""
| biobb_template Template
| Short description for the `template <http://templatedocumentation.org>`_ module in Restructured Text (reST) syntax. Mandatory.
| Long description for the `template <http://templatedocumentation.org>`_ module in Restructured Text (reST) syntax. Optional.

Args:        
    input_file_path1 (str): Description for the first input file path. File type: input. `Sample file <https://urlto.sample>`_. Accepted formats: top (edam:format_3881).
    input_file_path2 (str) (Optional): Description for the second input file path (optional). File type: input. `Sample file <https://urlto.sample>`_. Accepted formats: dcd (edam:format_3878).
    output_file_path (str): Description for the output file path. File type: output. `Sample file <https://urlto.sample>`_. Accepted formats: zip (edam:format_3987).
    properties (dic):
        * **boolean_property** (*bool*) - (True) Example of boolean property.
        * **binary_path** (*str*) - ("zip") Example of executable binary property.
        * **remove_tmp** (*bool*) - (True) [WF property] Remove temporal files.
        * **restart** (*bool*) - (False) [WF property] Do not execute if output files exist.

Examples:
    This is a use example of how to use the building block from Python::

        from biobb_template.template.template import template

        prop = { 
            'boolean_property': True 
        }
        template(input_file_path1='/path/to/myTopology.top',
                output_file_path='/path/to/newCompressedFile.zip',
                input_file_path2='/path/to/mytrajectory.dcd',
                properties=prop)

Info:
    * wrapped_software:
        * name: Zip
        * version: >=3.0
        * license: BSD 3-Clause
    * ontology:
        * name: EDAM
        * schema: http://edamontology.org/EDAM.owl

"""
```

The docstrings syntax is broadly explained in the [JSON Generator help](https://github.com/bioexcel/utils_biobb/tree/master/utils_biobb/json#docs-specifications) of the [utils_biobb](https://github.com/bioexcel/utils_biobb) repository. Following accurately this syntax, the [JSON Generator Tool](https://github.com/bioexcel/utils_biobb/blob/master/utils_biobb/json/json_generator.py) will automatically generate the JSON Schemas of the package.

All the multiline comments inside a set of triple quotes are used later as the function definition:


```python
def launch(self) -> int:
    """Execute the :class:`Template <template.template.Template>` object."""
```

### main() function

In the *main()* function we define the command line inputs and outputs and then we pass them to the launch function (described below) of the **Template** class.


```python
def main():
    """Command line execution of this building block. Please check the command line documentation."""
    parser = argparse.ArgumentParser(description='Description for the template module.', formatter_class=lambda prog: argparse.RawTextHelpFormatter(prog, width=99999))
    parser.add_argument('--config', required=False, help='Configuration file')

    # 10. Include specific args of each building block following the examples. They should match step 2
    required_args = parser.add_argument_group('required arguments')
    required_args.add_argument('--input_file_path1', required=True, help='Description for the first input file path. Accepted formats: top.')
    parser.add_argument('--input_file_path2', required=False, help='Description for the second input file path (optional). Accepted formats: dcd.')
    required_args.add_argument('--output_file_path', required=True, help='Description for the output file path. Accepted formats: zip.')

    args = parser.parse_args()
    args.config = args.config or "{}"
    properties = settings.ConfReader(config=args.config).get_prop_dic()

    # 11. Adapt to match Class constructor (step 2)
    # Specific call of each building block
    template(input_file_path1=args.input_file_path1, 
             output_file_path=args.output_file_path, 
             input_file_path2=args.input_file_path2,
             properties=properties)
```

### \_\_init\_\_() function

In the *\_\_init\_\_()* function initialises the **Template** class. In this function a dictionary with all the inputs and output (*self.io_dict*) is created, and all the properties are initialised.


```python
# 2. Adapt input and output file paths as required. Include all files, even optional ones
def __init__(self, input_file_path1, output_file_path, 
            input_file_path2 = None, properties = None, **kwargs) -> None:
    properties = properties or {}

    # 2.0 Call parent class constructor
    super().__init__(properties)
    self.locals_var_dict = locals().copy()

    # 2.1 Modify to match constructor parameters
    # Input/Output files
    self.io_dict = { 
        'in': { 'input_file_path1': input_file_path1, 'input_file_path2': input_file_path2 }, 
        'out': { 'output_file_path': output_file_path } 
    }

    # 3. Include all relevant properties here as 
    # self.property_name = properties.get('property_name', property_default_value)

    # Properties specific for BB
    self.boolean_property = properties.get('boolean_property', True)
    self.binary_path = properties.get('binary_path', 'zip')
    self.properties = properties

    # Check the properties
    self.check_properties(properties)
    # Check the arguments
    self.check_arguments()
```

### launch() function

In the function *launch()* we perform all the actions needed for the wrapping: creation of temporary folder(s), creation of command line, execution of command line and removing of the temporary folder(s). In this case we will comment all the blocks that shape this function separately.

#### @launchlogger decorator
Decorator used for wrapping the log. 


```python
@launchlogger
def launch(self) -> int:
    """Execute the :class:`Template <template.template.Template>` object."""
```

#### Setup BioBB
If *restart* property is enabled, skip this step. This property is only used for workflow purposes. Then, all files are staged.


```python
# 4. Setup Biobb
if self.check_restart(): return 0
self.stage_files()
```

#### Temporary folder
Creation of a temporary folder and copy the required input file inside it.


```python
# Creating temporary folder
self.tmp_folder = fu.create_unique_dir()
fu.log('Creating %s temporary folder' % self.tmp_folder, out_log)

# 5. Include here all mandatory input files
# Copy input_file_path1 to temporary folder
shutil.copy(self.io_dict['in']['input_file_path1'], self.tmp_folder)
```

#### Command line
Creation of command line call. If *boolean_property* is enabled, append **-v** option to command line.


```python
# 6. Prepare the command line parameters as instructions list
instructions = ['-j']
if self.boolean_property:
    instructions.append('-v')
    fu.log('Appending optional boolean property', self.out_log, self.global_log)

# 7. Build the actual command line as a list of items (elements order will be maintained)
self.cmd = [self.binary_path,
       ' '.join(instructions), 
       self.io_dict['out']['output_file_path'],
       str(PurePath(self.tmp_folder).joinpath(PurePath(self.io_dict['in']['input_file_path1']).name))]
fu.log('Creating command line with instructions and required arguments', self.out_log, self.global_log)
```

#### Optional input file

If optional input file provided, copy it to the temporary folder and append it to the command line call.


```python
# 8. Repeat for optional input files if provided
if self.io_dict['in']['input_file_path2']:
    # Copy input_file_path2 to temporary folder
    shutil.copy(self.io_dict['in']['input_file_path2'], self.tmp_folder)
    # Append optional input_file_path2 to cmd
    self.cmd.append(str(PurePath(self.tmp_folder).joinpath(PurePath(self.io_dict['in']['input_file_path2']).name)))
    fu.log('Appending optional argument to command line', self.out_log, self.global_log)
```

#### Launch execution


```python
# Run Biobb block
self.run_biobb()
```

#### Remove temporary file(s)
Remove temporary file(s) created during the execution. 

```python
# Remove temporary file(s)
self.tmp_files.extend([
    self.stage_io_dict.get("unique_dir"),
    self.tmp_folder
])
self.remove_tmp_files()
```

#### Check arguments
Check output arguments and warn the user if some output is incorrect or not created.

```python
# Check output arguments
self.check_arguments(output_files_created=True, raise_exception=False)
```

#### Return code
Finally, return *self.return_code* and finish the function.

```python
return self.return_code
```

### template() function

In the function *template()* we call the launch() function of the class Template. It will be used for external calls to the Template class such as Jupyter Notebooks and some adapters.


```python
def template(input_file_path1: str, output_file_path: str, input_file_path2: str = None, properties: dict = None, **kwargs) -> None:
"""Create :class:`Template <template.template.Template>` class and
execute the :meth:`launch() <template.template.Template.launch>` method."""

return Template(input_file_path1=input_file_path1, 
                output_file_path=output_file_path,
                input_file_path2=input_file_path2,
                properties=properties, **kwargs).launch()
```

## TemplateContainer class

Structure description for the **TemplateContainer** class of the *biobb_template/biobb_template/template/template_container.py* file. The complete source code is available in the [biobb_template repository](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template_container.py).

### Imports


```python
import argparse
from biobb_common.generic.biobb_object import BiobbObject
from biobb_common.configuration import  settings
from biobb_common.tools import file_utils as fu
from biobb_common.tools.file_utils import launchlogger
```

Libraries used for the execution of the **TemplateContainer** class.

### Class
Class template inherits from BiobbObject parent class.


```python
# 1. Rename class as required
class TemplateContainer(BiobbObject):
```

### Docstrings

The automatic documentation generation system through [**sphinx**](https://www.sphinx-doc.org/en/master/) is explained in detail in the [Documentation section](https://biobb-documentation.readthedocs.io/en/latest/documentation.html#formats-in-code-comments).

In the first line of the Template class we define all the arguments explained in the [Arguments](https://biobb-documentation.readthedocs.io/en/latest/arguments.html#arguments) section inside a set of triple quotes:


```python
"""
| biobb_template TemplateContainer
| Short description for the `template container <http://templatedocumentation.org>`_ module in Restructured Text (reST) syntax. Mandatory.
| Long description for the `template container <http://templatedocumentation.org>`_ module in Restructured Text (reST) syntax. Optional.

Args:
    input_file_path1 (str): Description for the first input file path. File type: input. `Sample file <https://urlto.sample>`_. Accepted formats: top (edam:format_3881).
    input_file_path2 (str) (Optional): Description for the second input file path (optional). File type: input. `Sample file <https://urlto.sample>`_. Accepted formats: dcd (edam:format_3878).
    output_file_path (str): Description for the output file path. File type: output. `Sample file <https://urlto.sample>`_. Accepted formats: zip (edam:format_3987).
    properties (dic):
        * **boolean_property** (*bool*) - (True) Example of boolean property.
        * **binary_path** (*str*) - ("zip") Example of executable binary property.
        * **remove_tmp** (*bool*) - (True) [WF property] Remove temporal files.
        * **restart** (*bool*) - (False) [WF property] Do not execute if output files exist.
        * **container_path** (*str*) - (None) Container path definition.
        * **container_image** (*str*) - ('mmbirb/zip:latest') Container image definition.
        * **container_volume_path** (*str*) - ('/tmp') Container volume path definition.
        * **container_working_dir** (*str*) - (None) Container working directory definition.
        * **container_user_id** (*str*) - (None) Container user_id definition.
        * **container_shell_path** (*str*) - ('/bin/bash') Path to default shell inside the container.

Examples:
    This is a use example of how to use the building block from Python::

        from biobb_template.template.template_container import template_container

        prop = { 
            'boolean_property': True,
            'container_path': 'docker',
            'container_image': 'mmbirb/zip:latest',
            'container_volume_path': '/tmp'
        }
        template_container(input_file_path1='/path/to/myTopology.top',
                        output_file_path='/path/to/newCompressedFile.zip',
                        input_file_path2='/path/to/mytrajectory.dcd',
                        properties=prop)

Info:
    * wrapped_software:
        * name: Zip
        * version: >=3.0
        * license: BSD 3-Clause
    * ontology:
        * name: EDAM
        * schema: http://edamontology.org/EDAM.owl

"""
```

The docstrings syntax is broadly explained in the [JSON Generator help](https://github.com/bioexcel/utils_biobb/tree/master/utils_biobb/json#docs-specifications) of the [utils_biobb](https://github.com/bioexcel/utils_biobb) repository. Following accurately this syntax, the [JSON Generator Tool](https://github.com/bioexcel/utils_biobb/blob/master/utils_biobb/json/json_generator.py) will automatically generate the JSON Schemas of the package.

All the multiline comments inside a set of triple quotes are used later as the function definition:


```python
def launch(self) -> int:
    """Execute the :class:`TemplateContainer <template.template_container.TemplateContainer>` object."""
```

### main() function

In the *main()* function we define the command line inputs and outputs and then we pass them to the launch function (described below) of the **TemplateContainer** class.


```python
def main():
    """Command line execution of this building block. Please check the command line documentation."""
    parser = argparse.ArgumentParser(description='Description for the template container module.', formatter_class=lambda prog: argparse.RawTextHelpFormatter(prog, width=99999))
    parser.add_argument('--config', required=False, help='Configuration file')

    # 10. Include specific args of each building block following the examples. They should match step 2
    required_args = parser.add_argument_group('required arguments')
    required_args.add_argument('--input_file_path1', required=True, help='Description for the first input file path. Accepted formats: top.')
    parser.add_argument('--input_file_path2', required=False, help='Description for the second input file path (optional). Accepted formats: dcd.')
    required_args.add_argument('--output_file_path', required=True, help='Description for the output file path. Accepted formats: zip.')

    args = parser.parse_args()
    args.config = args.config or "{}"
    properties = settings.ConfReader(config=args.config).get_prop_dic()

    # 11. Adapt to match Class constructor (step 2)
    # Specific call of each building block
    template_container(input_file_path1=args.input_file_path1, 
                      output_file_path=args.output_file_path, 
                      input_file_path2=args.input_file_path2, 
                      properties=properties)
```

### \_\_init\_\_() function

In the *\_\_init\_\_()* function initialises the **TemplateContainer** class. In this function a dictionary with all the inputs and output (*self.io_dict*) is created, and all the properties are initialised.


```python
# 2. Adapt input and output file paths as required. Include all files, even optional ones
def __init__(self, input_file_path1, output_file_path, 
            input_file_path2 = None, properties = None, **kwargs) -> None:
    properties = properties or {}

    # 2.0 Call parent class constructor
    super().__init__(properties)
    self.locals_var_dict = locals().copy()

    # 2.1 Modify to match constructor parameters
    # Input/Output files
    self.io_dict = { 
        'in': { 'input_file_path1': input_file_path1, 'input_file_path2': input_file_path2 }, 
        'out': { 'output_file_path': output_file_path } 
    }

    # 3. Include all relevant properties here as 
    # self.property_name = properties.get('property_name', property_default_value)

    # Properties specific for BB
    self.boolean_property = properties.get('boolean_property', True)
    self.binary_path = properties.get('binary_path', 'zip')
    self.properties = properties

    # Check the properties
    self.check_properties(properties)
    # Check the arguments
    self.check_arguments()
```

### launch() function

In the function *launch()* we perform all the actions needed for the wrapping: creation of temporary folder(s), mapping of these temporary folder(s) to the container, creation of command line, execution of command line, retrieve of data from the container and removing of the temporary folder(s). In this case we will comment all the blocks that shape this function separately.

#### @launchlogger decorator
Decorator used for wrapping the log. 


```python
@launchlogger
def launch(self) -> int:
    """Execute the :class:`TemplateContainer <template.template_container.TemplateContainer>` object."""
```

#### Setup BioBB
If *restart* property is enabled, skip this step. This property is only used for workflow purposes. Then, all files are staged.


```python
# 4. Setup Biobb
if self.check_restart(): return 0
self.stage_files()
```

#### Command line
Creation of command line call. If *boolean_property* is enabled, append **-v** option to command line.


```python
# 5. Prepare the command line parameters as instructions list
instructions = ['-j']
if self.boolean_property:
    instructions.append('-v')
    fu.log('Appending optional boolean property', self.out_log, self.global_log)

# 6. Build the actual command line as a list of items (elements order will be maintained)
self.cmd = [self.binary_path,
       ' '.join(instructions), 
       self.stage_io_dict['out']['output_file_path'],
       self.stage_io_dict['in']['input_file_path1']]
fu.log('Creating command line with instructions and required arguments', self.out_log, self.global_log)
```

#### Optional input file

If optional input file provided append it to the command line call.


```python
# 7. Repeat for optional input files if provided
if self.stage_io_dict['in']['input_file_path2']:
    # Append optional input_file_path2 to cmd
    self.cmd.append(self.stage_io_dict['in']['input_file_path2'])
    fu.log('Appending optional argument to command line', self.out_log, self.global_log)
```

#### Launch execution

Creation of command line according to the *container_path* property and the rest of container-related properties. Then, launch execution.


```python
# Run Biobb block
self.run_biobb()
```

#### Output data retrieval

Copy output file(s) from the mapped *container_volume_path* inside the container to the definitive output path defined by the user in the arguments.


```python
# Copy files to host
self.copy_to_host()
```

#### Remove temporary file(s)
Remove temporary file(s) created during the execution. 


```python
# Remove temporary file(s)
self.tmp_files.extend([
    self.stage_io_dict.get("unique_dir"),
    self.tmp_folder
])
self.remove_tmp_files()
```

#### Check arguments
Check output arguments and warn the user if some output is incorrect or not created.

```python
# Check output arguments
self.check_arguments(output_files_created=True, raise_exception=False)
```

#### Return code
Finally, return *self.return_code* and finish the function.

```python
return self.return_code
```

### template_container() function

In the function *template_container()* we call the launch() function of the class TemplateContainer. It will be used for external calls to the Template class such as Jupyter Notebooks and some adapters.


```python
def template_container(input_file_path1: str, output_file_path: str, input_file_path2: str = None, properties: dict = None, **kwargs) -> None:
"""Create :class:`TemplateContainer <template.template_container.TemplateContainer>` class and
execute the :meth:`launch() <template.template_container.TemplateContainer.launch>` method."""

return TemplateContainer(input_file_path1=input_file_path1, 
                        output_file_path=output_file_path,
                        input_file_path2=input_file_path2,
                        properties=properties, **kwargs).launch()
```
