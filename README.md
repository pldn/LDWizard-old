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
| ETL          | an abbreviation for Extract data, Transform data, Load data. Which is used to describe pipelines that transform data from one type to another type. 	    |


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
 - The upload/input component, for uploading the to be transformed csv and a possible tranformation script.
 - Wizard GUI component, GUI components that will handle one or multiple transformation processes.
 - Download/export component, For downloading/exporting the linked data and transformation script to your local file system.
 - Upload/publish component, For uploading/publishing the linked data and transformation script on the web.

### 4.1 upload/input component

### 4.1.1 Description and Priority

### 4.1.2 Stimulus/Response Sequences

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

## 5. Other Nonfunctional Requirements

### 5.1 Performance Requirements

### 5.2 Safety Requirements

### 5.3 Security Requirements

### 5.4 Software Quality Attributes

### 5.5 User Documentation

## 6. Other Requirements
