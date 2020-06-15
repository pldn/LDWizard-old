<img src="/docs/img/LDWizard.png" align="right">

# LDWizard

A generic framework for creating Linked Data in one Spell.

# Software Requirements and Design Document (Preliminary)

## 1. Introduction
The advent of Linked Data is evident through the ever increasing activities within [cultural heritage](https://www.netwerkdigitaalerfgoed.nl/tag/linked-open-data/) and [social and economic history](https://stories.datalegend.net). However, for most part these initiatives still rely on expert technical knowledge regarding vocabularies and data-transformation. Confronted with the question, 'so how can we do that (Linked Data)?', when presenting the results from the [2019 Hack-a-LOD](https://hackalod.com/index.php/2019/12/24/teams-en-resultaten-2019/) to the commmunity in a local drugstore, Erwin Folmer sparked the idea for LDWizard. A limited, but useful first gauge at transposing data to Linked Data for non-experts, yet with the ability for experts to advance on the output by the LDWizard.

### 1.1 Purpose

To transpose a csv file in a simple yet meaningful way to Linked Data that can be expanded upon by more advanced LD converters such as [RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt), [RMLeditor](https://rml.io/tools/rmleditor/) and [CoW](https://github.com/clariah/cow/wiki). This document provides the descriptions and specifications for the LDwizard. This document specifies the framework on which instantiations of the LDwizard can be build. The specifications of this product are created in collaboration with,  

### 1.2 Document Conventions

### 1.3 Product Scope

### 1.4 References

| Abbreviation |   	                                                                                                                                                      |
|---	         |---	                                                                                                                                                      |
| csv        	 | Comma-separated values, a tabular format, used as non-proprietary format for tabular data. described in [RFC4180](https://tools.ietf.org/html/rfc4180)  	|
| ETL          | An abbreviation for Extract data, Transform data, Load data. Which is used to describe pipelines that transform data from one type to another type. 	    |
| baseIRI      | An IRI that forms the basis link, which can be expanded with an extra path-element that will point to a specific resource                                |


## 2.Overall Description

The LDWizard is designed to be a starting framework for transformations from csv to linked data. The LDwizard is designed to be the starting point of a generic pipeline that can be customized and expanded to cater specified needs in designated fields.  

### 2.1 Product Perspective

The goal of this product is to have a framework that can handle most of the basic transformations needed to transform csv into good linked data. Secondly the framework will be designed to be customizable and expandable. Such that developers and users can customize the framework to fit their domains.

### 2.2 Product Functions

### 2.3 Operating Environment

### 2.4 Design and Implementation Constraints

The LDWizard will be a clientside, browser only framework. This means that the framework will have limits based on the hardware and the software the users have installed and which browsers the users currently have.
To make sure the LDWizard can still perform. We will assume that the user will use the recent versions of the browsers available on their machine.

### 2.5 Assumptions and Dependencies

The LDWizard assumes that there are three different types of users of the LDWizard.
 - A general user, somebody who will use an instantiated product set up by developers and domain experts.
 - A Linked data specialist, somebody who will use an instantiated product set up by the developers to kick start the development of a ETL.
 - A developers and domain experts, people who will use the LDWizard framework to set up their own instantiated version of the LDWizard. Catered for their domain/specific usecase.

The LDWizard assumes that all three types of users have a general knowledge of linked data. We also assume that the developer will have a general knowledge about Javascript and/or typescript.

## 3. External Interface Requirements

### 3.1 User Interfaces

### 3.2 Software Interfaces

### 3.3 Communications Interface

## 4. System Features

The basic LDWizard will consist out of 4 basic components:
 - The upload/input component, for uploading the to be transformed csv and a possible transformation script.
 - Wizard GUI component, GUI components that will handle one or multiple transformation processes.
 - Download/export component, For downloading/exporting the linked data and transformation script to your local file system.
 - Upload/publish component, For uploading/publishing the linked data and transformation script on the web.

### 4.1 upload/input component

#### csv upload

For the csv upload we will need to make a choice about how we interpret a correct csv document. This is due to the ambiguity of a correct csv file. A csv file can have multiple different delimiter formats, e.g. "," or ";". This could occur natural if the user uses the Dutch notation for decimal numbers. When this happens, the csv split will differ from what is expected.

Secondly a csv can have multiple different methods for declaring strings with quotations marks, e.g. """ or "'". Finally there are also different Implementations for spaces at the beginning and the end of the fields. These can also be handled different from csv to csv.

For this problem there are three solutions, we can either declare that:
 - A correct csv document, is something that the developers from LDWizard decide.
 - A correct csv document, is something the implementer of an instantiated LDwizard will decide.
 - A correct csv document, is something the user of a specific instantiated LDWizard will decide on a limited basis.

I would recommend that we implement the second solution as leading. The domain expert that will help create the instantiation of the LDWizard will probably also know which csv template is leading the domain. The domain expert can also help if users have the incorrect csv, and help them transform the csv file.

We do not expect that a csv will always have a header line. If the file does not have a header file we should use a baseIRI + the letter of the column as the IRI for the predicate.

The LDWizard will follow the <https://www.w3.org/TR/trig/> specifications for the handling of special characters. The LDWizard will handle these special characters as errors.

##### Limitations

The second decision we should take is the size of the csv documents. Here two factors can be limiting for us in how large the size of the file can be. The performance of the conversion script, and the size of the document that can be handled in the browser, without significant performance loss.

To make sure we can handle both limits I would recommend using a file limit of 50 mb. If we notice that we can improve or enlarge one or both we could always improve it.

A final hard limitation would be the amount of columns, and a limit on the amount of rows. Let's set the limit for the amount of rows on 30, for now. As it is expected that this would not improve the usability of the LDWizard if we enlarge this number any further. But we can always decide different.
Let's set the amount of rows on 1.048.576, the same limit as excel for the amount of rows. With the same footnote as for the amount of columns.


### 4.1.1 Description and Priority

### 4.1.2 Stimulus/Response Sequences

Stimulus: the user uploads a correct csv document. <br>
Response: The continue/transform button will enable and the document will be stored in the browser memory.

Stimulus: The user uploads an incorrect document.<br>
Response: The user will get a error saying that the document is incorrect and show the location of the error.

Stimulus: The user uploads an correct csv document, with incorrect special characters according to the <https://www.w3.org/TR/trig/> specifications.<br>
Response: The user will get a error saying that the document is incorrect and show the location of the error.

Stimulus: The user uploads multiple csv documents.<br>
Response: The user will get a warning saying it can only upload a single csv document.

Stimulus: The user uploads a correct conversion script.<br>
Response: The script will be handled accordingly. The user will see a transform instead of a continue button.

Stimulus: The user uploads an incorrect script.<br>
Response: The user will get a warning that the script is incorrect.

### 4.1.3 Functional Requirements

### 4.2 wizard GUI component

### 4.2.1 Description and Priority

### 4.2.2 Stimulus/Response Sequences

### 4.2.3 Functional Requirements

### 4.3 Download/export component

### 4.3.1 Description and Priority

### 4.3.2 Stimulus/Response Sequences

### 4.3.3 Functional Requirements

### 4.4 Upload/publish component

### 4.4.1 Description and Priority

### 4.4.2 Stimulus/Response Sequences

### 4.4.3 Functional Requirements

### 4.5 ETL script && conversion bindings

Instead of developing a new ETL-tool we will use an existing ETL-tool to execute the transformation step. We decided to use a client based transformation tool [RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt) as the tool to transform the csv to RDF. The LDWizard uses this language due to it's expandability and ease of use.

Due to the limitations of the LDWizard, the ETL script inside the browser is limited to a max set of rows and columns. To use the ETL-script on a larger csv the LDWizard has an export button to export the ETL-script. This has two advantages.
 - To use the script the user designed in the LDWizard outside of the LDWizard.
 - To import to the ETL-script for a different csv in the LDWizard.

To make the use of the ETL-script more generic we will give the user the possibility to export the ETL-script into different languages. At the moment LDWizard will make it possible to export the ETL-script into ([RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt), [RMLeditor](https://rml.io/tools/rmleditor/) or [CoW](https://github.com/clariah/cow/wiki)) language.


#### RATT <-> COW



#### RATT <-> RML


### 4.5.1 Description and Priority

### 4.5.2 Stimulus/Response Sequences

### 4.5.3 Functional Requirements



## 5. Other Nonfunctional Requirements

### 5.1 Performance Requirements

### 5.2 Safety Requirements

### 5.3 Security Requirements

### 5.4 Software Quality Attributes

### 5.5 User Documentation

## 6. Other Requirements
