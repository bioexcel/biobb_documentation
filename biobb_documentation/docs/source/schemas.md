# Schemas

## JSON Schemas

[**JSON Schema**](https://json-schema.org/) is a vocabulary that allows you to **annotate** and **validate** JSON documents.

### Tools JSON Schemas

Every tool has its own **JSON Schema** with the next structure for the Template tool:

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/json_schemas/template.json](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/json_schemas/template.json)


```python
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "$id": "http://bioexcel.eu/biobb_template/json_schemas/1.0/template",
    "title": "Description for the template (http://templatedocumentation.org) module.",
    "type": "object",
    "required": [
        "input_file_path1",
        "output_file_path"
    ],
    "properties": {
        "input_file_path1": {
            "type": "string",
            "description": "Description for the first input file path",
            "filetype": "input",
            "sample": "https://urlto.sample",
            "enum": [
                ".*\\.top$"
            ]
        },
        "input_file_path2": {
            "type": "string",
            "description": "Description for the second input file path (optional)",
            "filetype": "input",
            "sample": "https://urlto.sample",
            "enum": [
                ".*\\.dcd$"
            ]
        },
        "output_file_path": {
            "type": "string",
            "description": "Description for the output file path",
            "filetype": "output",
            "sample": "https://urlto.sample",
            "enum": [
                ".*\\.zip$"
            ]
        },
        "properties": {
            "type": "object",
            "properties": {
                "boolean_property": {
                    "type": "boolean",
                    "default": true,
                    "description": "Example of boolean property."
                },
                "executable_binary_property": {
                    "type": "string",
                    "default": "zip",
                    "description": "Example of executable binary property."
                },
                "remove_tmp": {
                    "type": "boolean",
                    "default": true,
                    "description": "[WF property] Remove temporal files."
                },
                "restart": {
                    "type": "boolean",
                    "default": false,
                    "description": "[WF property] Do not execute if output files exist."
                }
            }
        }
    },
    "additionalProperties": false
}
```

Although it's not difficult to create a **JSON Schema** by your own, we provide a tool for generating them automatically:

[https://github.com/gbayarri/json_generator](https://github.com/gbayarri/json_generator)

Following the instructions of this [**json_generator**](https://github.com/gbayarri/json_generator) tool, especially those referred to the [**files structure**](https://github.com/gbayarri/json_generator#files-structure) and to the [**docs specifications**](https://github.com/gbayarri/json_generator#docs-specifications), the tool will generate these **JSON Schema** files for you.

Additionally, this script also generates the configuration files from the data taken from the *biobb_template/biobb_template/test/conf.yml* file. The script will generate a JSON config file for each module with *properties* defined in its parameters. More information about these configuration files in the [Execution section](https://biobb-documentation.readthedocs.io/en/latest/execution.html#config).

### Package JSON

Additionally, in the same **json_schemas** folder, there is a JSON file with information related with the **BioBB** package.

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/json_schemas/biobb_template.json](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/json_schemas/biobb_template.json)

This information is used for automate the maintenance of the [Source & Docs table](https://mmb.irbbarcelona.org/biobb/availability/source) in the **BioBB** [official website](https://mmb.irbbarcelona.org/biobb).

As you can see below, you need to provide a general description, the addresses to repositories and containers, tools' descriptions, dependencies and some descriptive keywords of the package:


```python
{
    "_id": "biobb_template",
    "desc": "Description for biobb_template package",
    "github": "https://github.com/bioexcel/biobb_template",
    "readthedocs": "https://biobb-template.readthedocs.io/en/latest/",
    "conda": "",
    "docker": "",
    "singularity": "",
    "version": "1.0.0",
    "tools" : [
        {
            "block" : "Template", 
            "tool" : "zip", 
            "desc" : "Description for the template module.",
            "exec" : "template"
        },
        {
            "block" : "TemplateContainer", 
            "tool" : "zip", 
            "desc" : "Description for the template_container module.",
            "exec" : "template_container"
        }
    ],
    "dep_pypi" : [
        "install_requires=['biobb_common==2.0.1']", 
        "python_requires='==3.6.*'"
    ], 
    "dep_conda" : [
        "python ==3.6.*", 
        "biobb_common ==2.0.1"
    ],
    "keywords" : [
        "template", 
        "keywords"
    ]
}
```

## Bioschemas

[**Bioschemas**](https://bioschemas.org) aims to improve the **Findability of data in the life sciences**. It does this by encouraging people in the life sciences to use [**Schema.org**](https://schema.org/) markup in their websites so that they are indexable by search engines and other services. [**Bioschemas**](https://bioschemas.org) encourages the consistent use of markup to ease the consumption of the contained markup across many sites. This structured information then makes it easier to **discover**, **collate**, and **analyse** distributed data.

Bioschemas is making two main contributions:

* Proposing [new types and properties](https://bioschemas.org/types) to [**Schema.org**](https://schema.org/) to allow for the description of life science resources.
* [Profiles](https://bioschemas.org/specifications) over the [**Schema.org**](https://schema.org/) types that identify the essential properties to use in describing a resource.

[**Bioschemas**](https://bioschemas.org) started as a community effort in November 2015. It operates as an open community initiative with representatives from a wide variety of institutions. You are welcome to join the community.

### Schema.org

[**Schema.org**](https://schema.org/) is a collaborative, community activity with a mission to create, maintain, and promote schemas for structured data on the Internet, on web pages, in email messages, and beyond.

[**Schema.org**](https://schema.org/) vocabulary can be used with many different encodings, including **RDFa**, **Microdata** and **JSON-LD**. These vocabularies cover entities, relationships between entities and actions, and can easily be extended through a well-documented extension model. Over 10 million sites use Schema.org to markup their web pages and email messages. Many applications from Google, Microsoft, Pinterest, Yandex and others already use these vocabularies to power rich, extensible experiences.

Founded by Google, Microsoft, Yahoo and Yandex, Schema.org vocabularies are developed by an open community process, using the [public-schemaorg@w3.org](http://lists.w3.org/Archives/Public/public-schemaorg) mailing list and through [GitHub](http://github.com/schemaorg/schemaorg).

### BioBB schema

[https://github.com/bioexcel/biobb_template/blob/master/biobb_template/docs/source/schema.html](https://github.com/bioexcel/biobb_template/blob/master/biobb_template/docs/source/schema.html)

The **BioBB** HTML schema is in the *docs/source/* folder. The basic code is the next one:

```html
<script type="application/ld+json">
{
  "@context": "http://schema.org",
  "@type": "SoftwareApplication",
  "description": "Biobb_template is the Biobb module collection to perform molecular dynamics simulations.",
  "name": "BioBB Template",
  "url": "https://github.com/bioexcel/biobb_template",
  "additionalType": "Library",
  "applicationCategory": "Computational Biology tool",
  "applicationSubCategory": "http://www.edamontology.org/topic_3892",
  "citation": "https://www.nature.com/articles/s41597-019-0177-4",
  "license": "https://www.apache.org/licenses/LICENSE-2.0",
  "softwareVersion": "1.0.0",
  "applicationSuite": "BioBB BioExcel Building Blocks",
  "codeRepository": "https://github.com/bioexcel/biobb_template",
  "isAccessibleForFree": "True",
  "image": "http://mmb.irbbarcelona.org/biobb/public/assets/layouts/layout3/img/logo.png",
  "operatingSystem": ["Linux", "MacOS"],
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency":"EUR"
  }
}
</script>
```
