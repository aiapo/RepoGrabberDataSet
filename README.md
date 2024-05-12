# RepoGrabberDataSet
A compressed refactoring dataset file.

RepoGrabberDataSet (RGDS) is a GZip compressed dataset containing all the information that the tool [RepoGrabber](https://github.com/aiapo/RepoGrabber) collects.

[RepoGrabberGrapher](https://github.com/aiapo/RepoGrabberGrapher) takes input of a RGDS dataset to compute visualizations.

As a reference, a dataset that totaled 4,437 distinct repositories with 18,402,387 refactorings total was ~15GB in the database, whereas when exported to RGDS it only was ~4.5GB with all data included.

A **dataset.yaml** file located at the top-level directory is used to indicate what datasets may be imported, and where they are located in relation.

# Design
It contains information about the repositories, languages, and refactorings all in one file to be easily imported and shared.

The key way that this dataset file is designed is via the usage of Actions via the '@' symbol. Any word beginning with '@' is a runnable action.

Any line starting with '%' is a comment and is typically ignored in data reading, as comments are more for human information.

It is prefered to use double quotation marks to contain data in order to not cause potential issues.

# Actions
### `@RGDS_VERSION` [version: str]
The version number of the RGDS structure (currently 1.0.0).
Useful if the structure changes in the future

### `@TITLE` [title: str]
The title of the dataset to help differentiate between different sets.

### `@RELATION` [relationName: str]
The relation's name.

### `@ATTRIBUTE` [attributeName: str, attributeDataType: str]
The relation's attributes, each attribute gets it's own @ATTRIBUTE.

### `@DATA`
Indicates that data follows to start data reading.
Each line of data should be on it's own line, and values in a row should be comma delimited.

# Example
```
@RGDS_VERSION 1.0.0
% Title: Example dataset
% 
% Description: This dataset is for the
% purposes of demonstrating the RGDS format

@TITLE "Example dataset"

@RELATION ExampleRelation1
@ATTRIBUTE id integer
@ATTRIBUTE name varchar
@DATA

"1","EXAMPLEDATA"

@RELATION ExampleRelation2
@ATTRIBUTE id integer
@ATTRIBUTE zone integer
@DATA

"1","12345"
```

You may also view and test a small 2.7MB dataset with actual data located in [examples](https://github.com/aiapo/RepoGrabberDataSet/blob/main/examples/example.rgds).

# Multiple datasets and versions through `dataset.yaml`
This YAML file signifies where the datasets are located for easy imports. As an example by pointing a tool like RepoGrabberGrapher to a GitHub project, like this one, the tool will be able to find all the wanted to be imported datasets.

If using, this file must be put at the directory to be entered in at prompt. For example if the user wants to import this repositority's samples into RepoGrabberGrapher they will enter `https://github.com/aiapo/RepoGrabberDataSet` which will access `dataset.yaml` at the top directory to grab the RGDS files.

The layout of this YAML file is as specifed:

```
---
title: TITLE OF ALL DATASETS
description: DESCRIPTION OF ALL DATASETS
datasets:
  - title: TITLE OF SPECIFIC DATASET
    description: DESCRIPTION OF SPECIFIC DATASET
    created: CREATION DATE OF SPECIFIC DATASET
    import: (TRUE/FALSE) IF THE DATASET SHOULD BE IMPORTED
    location: LOCATION IN RELATION TO THE YAML FILE
```

# Thanks
Design of the file inspired by [Weka ARFF](https://waikato.github.io/weka-wiki/formats_and_processing/arff_stable/)