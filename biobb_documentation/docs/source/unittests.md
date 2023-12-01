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
        returncode = template(properties=self.properties, **self.paths)
        assert fx.not_empty(self.paths['output_file_path'])
        assert fx.equal(self.paths['output_file_path'], self.paths['ref_output_file_path'])
        assert fx.exe_success(returncode)
```

Example of [test_template_container.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/test/unitests/test_template/test_template_container.py), that performs the unittest for the [template_container.py](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/template/template_container.py) code, in this case one test for **docker container** and another for **singularity container**:


```python
import pytest
from biobb_common.tools import test_fixtures as fx
from biobb_template.template.template_container import template_container


@pytest.mark.skip(reason="skip containers when testing")
class TestTemplateDocker():
    def setup_class(self):
        fx.test_setup(self, 'template_docker')

    def teardown_class(self):
        fx.test_teardown(self)
        pass

    def test_template_docker(self):
        returncode = template_container(properties=self.properties, **self.paths)
        assert fx.not_empty(self.paths['output_file_path'])
        assert fx.equal(self.paths['output_file_path'], self.paths['ref_output_file_path'])
        assert fx.exe_success(returncode)


@pytest.mark.skip(reason="skip containers when testing")
class TestTemplateSingularity():
    def setup_class(self):
        fx.test_setup(self, 'template_singularity')

    def teardown_class(self):
        fx.test_teardown(self)
        pass

    def test_template_singularity(self):
        returncode = template_container(properties=self.properties, **self.paths)
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

template_docker:
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

## GitHub Actions

The unittests can be run automatically after pushing a commit to GitHub through the [**GitHub Actions**](https://github.com/features/actions) feature. The [BioExcel](https://github.com/bioexcel) official repository (that contains all the BioBB packages repositories) has been configured in order to launch testing after pushing some repository containing the [.github](https://biobb-documentation.readthedocs.io/en/latest/files_structure.html#github-folder) folder included in the **biobb_template**.

In this **.github** folder there are only two YAML files:

### env.yaml

File with all the **dependencies** needed for running the biobb package in a conda **environment**:

```yaml
name: test_environment
channels:
  - conda-forge
  - bioconda
  - anaconda
dependencies:
  - biobb_common ==4.1.0
  - zip
```

### linting_and_testing.yml

This file is the **workflow** that runs the tests in an external Virtual Machine configured to run automatically through **GitHub actions**:

```yaml
name: tests

on: 
  # workflow_dispatch
  push:
    branches: [ master ]
    paths-ignore:
      - '.gitignore'
      - '.readthedocs.yaml'
      - 'LICENSE'
      - 'setup.py'
      - 'README.md'
      - '**/docs/**'
      - '**/json_schemas/**'

jobs:
  # Name of the Job
  lint_and_test:
    strategy:
      matrix:
        os: [self-hosted]
        python-version: ["3.8", "3.9", "3.10"]
    runs-on: ${{ matrix.os }}
    steps:
      - name: Check out repository code
        uses: actions/checkout@v3

      - run: echo "Repository -> ${{ github.repository }}"
      - run: echo "Branch -> ${{ github.ref }}"
      - run: echo "Trigger event -> ${{ github.event_name }}"
      - run: echo "Runner OS -> ${{ runner.os }}"

      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}

      - name: Remove all micromamba installations
        run: |
          rm -rf /home/user/.bash_profile /home/user/.conda /home/user/micromamba /home/user/micromamba-bin 2>/dev/null
          touch /home/user/.bash_profile

      - name: provision-with-micromamba
        uses: mamba-org/setup-micromamba@v1
        with:
          generate-run-shell: true
          post-cleanup: all
          environment-file: .github/env.yaml
          create-args: >-
            python=${{ matrix.python-version }}
            pytest
            pytest-cov
            pytest-html
            flake8
            pip
      
      - name: Install genbadge from pip
        shell: micromamba-shell {0}  # necessary for conda env to be active
        run: pip install genbadge[all]

      - name: List installed package versions
        shell: micromamba-shell {0}  # necessary for conda env to be active
        run: micromamba list

      - name: Lint with flake8
        shell: micromamba-shell {0}  # necessary for conda env to be active
        run: |
          # F Codes: https://flake8.pycqa.org/en/latest/user/error-codes.html
          # E Code: https://pycodestyle.pycqa.org/en/latest/intro.html#error-codes

          # Workflow fails: Stop the build if there are Python syntax errors or undefined names
          flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
          
          # Create directory for flake8 reports
          mkdir -p ./reports/flake8

          # Exit-zero treats all errors as warnings, workflow will not fail:
          flake8 . --exclude=docs --ignore=C901,E226,W605 --count --exit-zero --max-complexity=10 --max-line-length=9999 --statistics --format=html --htmldir=./reports/flake8/ --tee --output-file=./reports/flake8/flake8stats.txt
      
      - name: Generate Flake8 badge
        shell: micromamba-shell {0}  # necessary for conda env to be active
        run: |
          genbadge flake8 --name "Flake8" --input-file ./reports/flake8/flake8stats.txt  --output-file ./reports/flake8/flake8badge.svg

      - name: Checkout biobb_common
        uses: actions/checkout@v3
        with:
          repository: bioexcel/biobb_common
          path: './biobb_common'

      - name: Run tests
        shell: micromamba-shell {0}  # necessary for conda env to be active
        run: |
          # Ignoring docker and singularity tests
          export PYTHONPATH=.:./biobb_common:$PYTHONPATH
          
          # Create directory for tests reports
          mkdir -p ./reports/junit

          # Producction one
          pytest biobb_template/test/unitests/ --cov=biobb_template/ --cov-report=xml --junit-xml=./reports/junit/junit.xml --html=./reports/junit/report.html

      - name: Generate Tests badge
        shell: micromamba-shell {0}  # necessary for conda env to be active
        run: |
          genbadge tests --name "Tests" --input-file ./reports/junit/junit.xml  --output-file ./reports/junit/testsbadge.svg

      - name: Generate Coverage badge
        shell: micromamba-shell {0}  # necessary for conda env to be active
        run: |
          # Create directory for flake8 reports
          mkdir -p ./reports/coverage

          coverage xml -o ./reports/coverage/coverage.xml
          coverage html -d ./reports/coverage/

          genbadge coverage --name "Coverage" --input-file ./reports/coverage/coverage.xml  --output-file ./reports/coverage/coveragebadge.svg

      - name: Publish coverage report to GitHub Pages
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          folder: ./reports
```

This **workflow** has been configured for linting and testing all BioBB's, so the **only part that must be customized** is the _Run tests_ step, where the unittests explained above in this same section are run:

```yaml
- name: Run tests
  shell: micromamba-shell {0}  # necessary for conda env to be active
  run: |
    # Ignoring docker and singularity tests
    export PYTHONPATH=.:./biobb_common:$PYTHONPATH
    
    # Create directory for tests reports
    mkdir -p ./reports/junit

    # Producction one
    pytest biobb_template/test/unitests/ --cov=biobb_template/ --cov-report=xml --junit-xml=./reports/junit/junit.xml --html=./reports/junit/report.html
```

For the sake of the efficiency, the **container** version of the tools **won't be tested**.