# Unittests

In computer programming, **unit testing** is a software testing method by which individual units of source code, sets of one or more computer program modules together with associated control data, usage procedures, and operating procedures, are **tested** to determine whether they are fit for use.

The **BioBB unittests** are performed using [pytest](https://docs.pytest.org/en/7.1.x/), that comes installed in the conda environment generated in the *biobb_template*.

## Files structure

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test)

Opening the *test* folder in *biobb_template* shows us the next files structure:

* data/
    * config/
        * config_template.json
        * config_template.yml
        * config_template_container.json
        * config_template_container.yml
        * config_template_singularity.json
        * config_template_singularity.yml
    * template/
        * topology.top
        * trajectory.dcd
* reference/
    * template/
        * output_container.zip
        * output.zip
* unitests/
    * test_template/
        * test_template.py
        * test_template_container.py
* \_\_init\_\_.py
* conf.yml

### data folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/data](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/data)

In this folder we find two subfolders:

#### config folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/data/config](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/data/config)

These config **JSON and YAML files** are automatically generated as explained in the [JSON Schemas section](https://biobb-documentation.readthedocs.io/en/latest/schemas.html#tools-json-schemas). Its utility is mainly for the [BioBB REST API](https://mmb.irbbarcelona.org/biobb-api/) purposes and for the automatic generation of the [Command Line Documentation](https://biobb-documentation.readthedocs.io/en/latest/documentation.html#command-line-documentation). They are generated after parsing the *conf.yml* file explained below in this same section.

Example of [config_template.json](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/test/data/config/config_template.json):


```python
{
    "properties": {
        "boolean_property": false,
        "remove_tmp": true
    }
}
```

Example of [config_template.yml](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/test/data/config/config_template.yml):

```yml
properties:
  boolean_property: false
  remove_tmp: true
```

#### template folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/data/template](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/data/template)

In this folder we find the two input files needed for the execution of the two tools (Template and TemplateContainer).

### reference folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/reference](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/reference)

#### template folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/reference/template](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/reference/template)

In this folder we find the two input files generated as a result of the execution of the two tools (Template and TemplateContainer).

### unitests folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/unitests](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/unitests)

#### test_template folder

[https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/unitests/test_template](https://github.com/bioexcel/biobb_template/tree/master/biobb_template/test/unitests/test_template)

In this folder we find the two Python files used for the test execution. They parse the *conf.yml* file explained below and execute the unittest.

Example of [test_template.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/test/unitests/test_template/test_template.py), that performs the unittest for the [template.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template.py) code:


```python
from biobb_common.tools import test_fixtures as fx
from biobb_template.template.template import template

class TestTemplate():
    def setup_class(self):
        fx.test_setup(self, 'template')

    def teardown_class(self):
        fx.test_teardown(self)
        pass

    def test_template(self):
        returncode= template(properties=self.properties, **self.paths)
        assert fx.not_empty(self.paths['output_file_path'])
        assert fx.equal(self.paths['output_file_path'], self.paths['ref_output_file_path'])
        assert fx.exe_success(returncode)
```

Example of [test_template_container.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/test/unitests/test_template/test_template_container.py), that performs the unittest for the [template_container.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template_container.py) code, in this case one test for **docker container** and another for **singularity container**:


```python
from biobb_common.tools import test_fixtures as fx
from biobb_template.template.template_container import template_container

class TestTemplateDocker():
    def setup_class(self):
        fx.test_setup(self, 'template_container')

    def teardown_class(self):
        fx.test_teardown(self)
        pass

    def test_template_docker(self):
        returncode= template_container(properties=self.properties, **self.paths)
        assert fx.not_empty(self.paths['output_file_path'])
        assert fx.equal(self.paths['output_file_path'], self.paths['ref_output_file_path'])
        assert fx.exe_success(returncode)

class TestTemplateSingularity():
    def setup_class(self):
        fx.test_setup(self, 'template_singularity')

    def teardown_class(self):
        fx.test_teardown(self)
        pass

    def test_template_singularity(self):
        returncode= template_container(properties=self.properties, **self.paths)
        assert fx.not_empty(self.paths['output_file_path'])
        assert fx.equal(self.paths['output_file_path'], self.paths['ref_output_file_path'])
        assert fx.exe_success(returncode)
```

### conf.yml

**YAML** file with all the paths and properties for **unittests**. For each test we must define:

* paths
    * input paths: paths to the files defined in the *test/data/template* folder.
    * output paths: name for the file that will be generated after the execution, usuarlly in the */tmp* folder.
    * reference output paths: paths to the files defined in the *test/reference/template* folder.
* properties

The *biobb_template* [conf.yml](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/test/conf.yml) file:

```yml
working_dir_path: /tmp/biobb/unitests

template:
  paths:
    input_file_path1: file:test_data_dir/template/topology.top
    input_file_path2: file:test_data_dir/template/trajectory.dcd
    output_file_path: output.zip
    ref_output_file_path: file:test_reference_dir/template/output.zip
  properties:
    boolean_property: false
    remove_tmp: true

template_container:
  paths:
    input_file_path1: file:test_data_dir/template/topology.top
    input_file_path2: file:test_data_dir/template/trajectory.dcd
    output_file_path: output.zip
    ref_output_file_path: file:test_reference_dir/template/output.container.zip
  properties:
    boolean_property: false
    remove_tmp: true
    container_path: docker
    container_image: mmbirb/zip:latest
    container_volume_path: /tmp

template_singularity:
  paths:
    input_file_path1: file:test_data_dir/template/topology.top
    input_file_path2: file:test_data_dir/template/trajectory.dcd
    output_file_path: output.zip
    ref_output_file_path: file:test_reference_dir/template/output.container.zip
  properties:
    boolean_property: false
    remove_tmp: false
    binary_path: /opt/conda/bin/zip
    container_path: singularity
    container_image: bioexcel-zip_container-master-latest.simg
    container_volume_path: /tmp
```

## Execution

Finally, in order to execute the unittests, you only need to call the Python test files through [pytest](https://docs.pytest.org/en/7.1.x/):

### Template

```Shell
pytest -s biobb_template/biobb_template/test/unitests/test_template/test_template.py
```

### Template Container

```Shell
pytest -s biobb_template/biobb_template/test/unitests/test_template/test_template_container.py
```
