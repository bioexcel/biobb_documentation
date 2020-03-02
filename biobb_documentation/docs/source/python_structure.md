
# Python structure

In this section we will describe the basic structure of the wrapped tool python files.

## Template class

Structure description for the **Template** class of the *biobb_template/biobb_template/template/template.py* file. The complete source code is available in the [biobb_template repository](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template.py).

### Imports


```python
import argparse
import shutil
from pathlib import Path, PurePath
from biobb_common.configuration import  settings
from biobb_common.tools import file_utils as fu
from biobb_common.tools.file_utils import launchlogger
from biobb_common.command_wrapper import cmd_wrapper
```

Libraries used for the execution of the **Template** class.

### Comments

The automatic documentation generation system through [**sphinx**](https://www.sphinx-doc.org/en/master/) is explained in detail in the [Documentation section](https://biobb-documentation.readthedocs.io/en/latest/documentation.html).

In the first line of the Template class we define all the arguments explained in the [Arguments](https://biobb-documentation.readthedocs.io/en/latest/arguments.html) section inside a set of triple quotes:


```python
"""Description for the template (http://templatedocumentation.org) module.

Args:
    input_file_path1 (str): Description for the first input file path. File type: input. `Sample file <https://urlto.sample>`_. Accepted formats: top.
    input_file_path2 (str) (Optional): Description for the second input file path (optional). File type: input. `Sample file <https://urlto.sample>`_. Accepted formats: dcd.
    output_file_path (str): Description for the output file path. File type: output. `Sample file <https://urlto.sample>`_. Accepted formats: zip.
    properties (dic):
        * **boolean_property** (*bool*) - (True) Example of boolean property.
        * **executable_binary_property** (*str*) - ("zip") Example of executable binary property.
        * **remove_tmp** (*bool*) - (True) [WF property] Remove temporal files.
        * **restart** (*bool*) - (False) [WF property] Do not execute if output files exist.
"""
```

All the multiline comments inside a set of triple quotes are used later as the function definition:


```python
def launch(self):
    """Launches the execution of the template module."""
```

### main() function

In the *main()* function we define the command line inputs and outputs and then we pass them to the launch function (described below) of the **Template** class.


```python
def main():
    parser = argparse.ArgumentParser(description='Description for the template module.', formatter_class=lambda prog: argparse.RawTextHelpFormatter(prog, width=99999))
    parser.add_argument('--config', required=False, help='Configuration file')

    # 10. Include specific args of each building block following the examples. They should match step 2
    required_args = parser.add_argument_group('required arguments')
    required_args.add_argument('--input_file_path1', required=True, help='Description for the first input file path. Accepted formats: top.')
    parser.add_argument('--input_file_path2', required=False, help='Description for the second input file path (optional). Accepted formats: dcd.')
    required_args.add_argument('--output_file_path', required=True, help='Description for the output file path. Accepted formats: zip.')

    args = parser.parse_args()
    config = args.config if args.config else None
    properties = settings.ConfReader(config=config).get_prop_dic()

    # 11. Adapt to match Class constructor (step 2)
    # Specific call of each building block
    Template(input_file_path1=args.input_file_path1, input_file_path2=args.input_file_path2, 
             output_file_path=args.output_file_path, 
             properties=properties).launch()
```

### \_\_init\_\_() function

In the *\_\_init\_\_()* function initialises the **Template** class. In this function a dictionary with all the inputs and output (*self.io_dict*) is created, and all the properties are initialised.


```python
# 2. Adapt input and output file paths as required. Include all files, even optional ones
def __init__(self, input_file_path1, input_file_path2, output_file_path, properties, **kwargs):
    properties = properties or {}

    # 2.1 Modify to match constructor parameters
    # Input/Output files
    self.io_dict = { 
        'in': { 'input_file_path1': input_file_path1, 'input_file_path2': input_file_path2 }, 
        'out': { 'output_file_path': output_file_path } 
    }

    # 3. Include all relevant properties here as 
    # self.property_name = properties.get('property_name', property_default_value)

    # Properties specific for BB
    self.properties = properties
    self.boolean_property = properties.get('boolean_property', True)
    self.executable_binary_property = properties.get('executable_binary_property', 'zip')

    # Properties common in all BB
    self.can_write_console_log = properties.get('can_write_console_log', True)
    self.global_log = properties.get('global_log', None)
    self.prefix = properties.get('prefix', None)
    self.step = properties.get('step', None)
    self.path = properties.get('path', '')
    self.remove_tmp = properties.get('remove_tmp', True)
    self.restart = properties.get('restart', False)
```

### launch() function

In the function *launch()* we perform all the actions needed for the wrapping: creation of temporary folder(s), creation of command line, execution of command line and removing of the temporary folder(s). In this case we will comment all the blocks that shape this function separately.

#### @launchlogger decorator
Decorator used for wrapping the log. 


```python
@launchlogger
def launch(self):
    """Launches the execution of the template module."""
```

#### Loggers definition
Definition of local loggers from launchlogger decorator.


```python
# Get local loggers from launchlogger decorator
out_log = getattr(self, 'out_log', None)
err_log = getattr(self, 'err_log', None)
```

#### Properties checking
Check if provided properties match with the ones defined for this tool.


```python
# Check the properties
fu.check_properties(self, self.properties)
```

#### Restart
If *restart* property is enabled, skip this step. This property is only used for workflow purposes.


```python
# Restart
if self.restart:
    # 4. Include here all output file paths
    output_file_list = [self.io_dict['out']['output_file_path']]
    if fu.check_complete_files(output_file_list):
        fu.log('Restart is enabled, this step: %s will the skipped' % self.step, out_log, self.global_log)
        return 0
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
    fu.log('Appending optional boolean property', out_log, self.global_log)

# 7. Build the actual command line as a list of items (elements order will be maintained)
cmd = [self.executable_binary_property,
       ' '.join(instructions), 
       self.io_dict['out']['output_file_path'],
       str(PurePath(self.tmp_folder).joinpath(PurePath(self.io_dict['in']['input_file_path1']).name))]
fu.log('Creating command line with instructions and required arguments', out_log, self.global_log)
```

#### Optional input file

If optional input file provided, copy it to the temporary folder and append it to the command line call.


```python
# 8. Repeat for optional input files if provided
if self.io_dict['in']['input_file_path2']:
    # Copy input_file_path2 to temporary folder
    shutil.copy(self.io_dict['in']['input_file_path2'], self.tmp_folder)
    # Append optional input_file_path2 to cmd
    cmd.append(str(PurePath(self.tmp_folder).joinpath(PurePath(self.io_dict['in']['input_file_path2']).name)))
    fu.log('Appending optional argument to command line', out_log, self.global_log)
```

#### Launch execution


```python
# Launch execution
returncode = cmd_wrapper.CmdWrapper(cmd, out_log, err_log, self.global_log).launch()
```

#### Remove temporary file(s)
If *remove_tmp* is enabled, remove temporary file(s) created during the execution. Then, return *returncode* and finish the function.


```python
# Remove temporary file(s)
if self.remove_tmp: 
    fu.rm(self.tmp_folder)
    fu.log('Removed: %s' % str(self.tmp_folder), out_log)
    
return returncode
```

## TemplateContainer class

Structure description for the **TemplateContainer** class of the *biobb_template/biobb_template/template/template_container.py* file. The complete source code is available in the [biobb_template repository](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template_container.py).

### Imports


```python
import argparse
import shutil
from pathlib import Path, PurePath
from biobb_common.configuration import  settings
from biobb_common.tools import file_utils as fu
from biobb_common.tools.file_utils import launchlogger
from biobb_common.command_wrapper import cmd_wrapper
```

Libraries used for the execution of the **TemplateContainer** class.

### Comments

The automatic documentation generation system through [**sphinx**](https://www.sphinx-doc.org/en/master/) is explained in detail in the [Documentation section](https://biobb-documentation.readthedocs.io/en/latest/documentation.html).

In the first line of the Template class we define all the arguments explained in the [Arguments](https://biobb-documentation.readthedocs.io/en/latest/arguments.html) section inside a set of triple quotes:


```python
"""Description for the template container (http://templatedocumentation.org) module.

Args:
    input_file_path1 (str): Description for the first input file path. File type: input. `Sample file <https://urlto.sample>`_. Accepted formats: top.
    input_file_path2 (str) (Optional): Description for the second input file path (optional). File type: input. `Sample file <https://urlto.sample>`_. Accepted formats: dcd.
    output_file_path (str): Description for the output file path. File type: output. `Sample file <https://urlto.sample>`_. Accepted formats: zip.
    properties (dic):
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
"""
```

All the multiline comments inside a set of triple quotes are used later as the function definition:


```python
def launch(self):
    """Launches the execution of the template_container module."""
```

### main() function

In the *main()* function we define the command line inputs and outputs and then we pass them to the launch function (described below) of the **TemplateContainer** class.


```python
def main():
    parser = argparse.ArgumentParser(description='Description for the template module.', formatter_class=lambda prog: argparse.RawTextHelpFormatter(prog, width=99999))
    parser.add_argument('--config', required=False, help='Configuration file')

    # 11. Include specific args of each building block following the examples. They should match step 2
    required_args = parser.add_argument_group('required arguments')
    required_args.add_argument('--input_file_path1', required=True, help='Description for the first input file path. Accepted formats: top.')
    parser.add_argument('--input_file_path2', required=False, help='Description for the second input file path (optional). Accepted formats: dcd.')
    required_args.add_argument('--output_file_path', required=True, help='Description for the output file path. Accepted formats: zip.')

    args = parser.parse_args()
    config = args.config if args.config else None
    properties = settings.ConfReader(config=config).get_prop_dic()

    # 12. Adapt to match Class constructor (step 2)
    # Specific call of each building block
    TemplateContainer(input_file_path1=args.input_file_path1, input_file_path2=args.input_file_path2, 
                      output_file_path=args.output_file_path, 
                      properties=properties).launch()
```

### \_\_init\_\_() function

In the *\_\_init\_\_()* function initialises the **TemplateContainer** class. In this function a dictionary with all the inputs and output (*self.io_dict*) is created, and all the properties are initialised.


```python
# 2. Adapt input and output file paths as required. Include all files, even optional ones
def __init__(self, input_file_path1, input_file_path2, output_file_path, properties, **kwargs):
    properties = properties or {}

    # 2.1 Modify to match constructor parameters
    # Input/Output files
    self.io_dict = { 
        'in': { 'input_file_path1': input_file_path1, 'input_file_path2': input_file_path2 }, 
        'out': { 'output_file_path': output_file_path } 
    }

    # 3. Include all relevant properties here as 
    # self.property_name = properties.get('property_name', property_default_value)

    # Properties specific for BB
    self.properties = properties
    self.boolean_property = properties.get('boolean_property', True)
    self.executable_binary_property = properties.get('executable_binary_property', 'zip')

    # container Specific
    self.container_path = properties.get('container_path')
    self.container_image = properties.get('container_image', 'mmbirb/zip:latest')
    self.container_volume_path = properties.get('container_volume_path', '/tmp')
    self.container_working_dir = properties.get('container_working_dir')
    self.container_user_id = properties.get('container_user_id')
    self.container_shell_path = properties.get('container_shell_path', '/bin/bash')

    # Properties common in all BB
    self.can_write_console_log = properties.get('can_write_console_log', True)
    self.global_log = properties.get('global_log', None)
    self.prefix = properties.get('prefix', None)
    self.step = properties.get('step', None)
    self.path = properties.get('path', '')
    self.remove_tmp = properties.get('remove_tmp', True)
    self.restart = properties.get('restart', False)
```

### launch() function

In the function *launch()* we perform all the actions needed for the wrapping: creation of temporary folder(s), mapping of these temporary folder(s) to the container, creation of command line, execution of command line, retrieve of data from the container and removing of the temporary folder(s). In this case we will comment all the blocks that shape this function separately.

#### @launchlogger decorator
Decorator used for wrapping the log. 


```python
@launchlogger
def launch(self):
    """Launches the execution of the template_container module."""
```

#### Loggers definition
Definition of local loggers from launchlogger decorator.


```python
# Get local loggers from launchlogger decorator
out_log = getattr(self, 'out_log', None)
err_log = getattr(self, 'err_log', None)
```

#### Properties checking
Check if provided properties match with the ones defined for this tool.


```python
# Check the properties
fu.check_properties(self, self.properties)
```

#### Restart
If *restart* property is enabled, skip this step. This property is only used for workflow purposes.


```python
# Restart
if self.restart:
    # 4. Include here all output file paths
    output_file_list = [self.io_dict['out']['output_file_path']]
    if fu.check_complete_files(output_file_list):
        fu.log('Restart is enabled, this step: %s will the skipped' % self.step, out_log, self.global_log)
        return 0
```

#### Copy inputs to container
Creation of a temporary folder and map it to the *container_volume_path* path.


```python
# 5. Copy inputs to container
container_io_dict = fu.copy_to_container(self.container_path, self.container_volume_path, self.io_dict)
```

#### Command line
Creation of command line call. If *boolean_property* is enabled, append **-v** option to command line.


```python
# 6. Prepare the command line parameters as instructions list
instructions = ['-j']
if self.boolean_property:
    instructions.append('-v')
    fu.log('Appending optional boolean property', out_log, self.global_log)

# 7. Build the actual command line as a list of items (elements order will be maintained)
cmd = [self.executable_binary_property,
       ' '.join(instructions), 
       container_io_dict['out']['output_file_path'],
       container_io_dict['in']['input_file_path1']]
fu.log('Creating command line with instructions and required arguments', out_log, self.global_log)
```

#### Optional input file

If optional input file provided append it to the command line call.


```python
# 8. Repeat for optional input files if provided
if container_io_dict['in']['input_file_path2']:
    # Append optional input_file_path2 to cmd
    cmd.append(container_io_dict['in']['input_file_path2'])
    fu.log('Appending optional argument to command line', out_log, self.global_log)
```

#### Launch execution

Creation of command line according to the *container_path* property and the rest of container-related properties. Then, launch execution.


```python
# 10. Create cmd with specdific syntax according to the required container
cmd = fu.create_cmd_line(cmd, container_path=self.container_path, 
                         host_volume=container_io_dict.get('unique_dir'), 
                         container_volume=self.container_volume_path, 
                         container_working_dir=self.container_working_dir, 
                         container_user_uid=self.container_user_id, 
                         container_image=self.container_image, 
                         container_shell_path=self.container_shell_path, 
                         out_log=out_log, global_log=self.global_log)

# Launch execution
returncode = cmd_wrapper.CmdWrapper(cmd, out_log, err_log, self.global_log).launch()
```

#### Output data retrieval

Copy output file(s) from the mapped *container_volume_path* inside the container to the definitive output path defined by the user in the arguments.


```python
# Copy output(s) to output(s) path(s) in case of container execution
fu.copy_to_host(self.container_path, container_io_dict, self.io_dict)
```

#### Remove temporary file(s)
If *remove_tmp* is enabled, remove temporary file(s) created during the execution. Then, return *returncode* and finish the function.


```python
# Remove temporary file(s)
if self.remove_tmp and container_io_dict.get('unique_dir'): 
    fu.rm(container_io_dict.get('unique_dir'))
    fu.log('Removed: %s' % str(container_io_dict.get('unique_dir')), out_log)

return returncode
```
