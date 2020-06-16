<img src="/docs/img/LDWizard.png" align="right">

# LDWizard

A generic framework for converting common datasources to Linked Data.

# Software Requirements and Design Document (Preliminary)

## 1. Introduction

The advent of Linked Data is evident through the ever increasing activities within [cultural heritage](https://www.netwerkdigitaalerfgoed.nl/tag/linked-open-data/) and [social and economic history](https://stories.datalegend.net). However, for most part these initiatives still rely on expert technical knowledge regarding vocabularies and data-transformation. Confronted with the question, 'so how can we do that (Linked Data)?', when presenting the results from the [2019 Hack-a-LOD](https://hackalod.com/index.php/2019/12/24/teams-en-resultaten-2019/) to the commmunity in a local drugstore, Erwin Folmer sparked the idea for LDWizard. A limited, but useful first gauge at transposing data to Linked Data for non-experts, yet with the ability for experts to advance on the output by the LDWizard.

### 1.1 Purpose

The purpose of LD Wizard is to transpose a CSV input file in a simple yet meaningful way in Linked Open Data.  LD Wizard specifically forcusses on establishing an initial transformation that requires no prior knowledge of Linked Data technologies to establish.  At the same time, LD Wizard allows this initial transformation to be exported into a widely used data transformation language.  This export can be used with more advanced Linked Data transformation tools in order to expand upon the initial transformation created with LD Wizard.

### 1.2 Document Conventions

### 1.3 Product Scope

### 1.4 References

## 2.Overall Description

### 2.1 Product Perspective

### 2.2 Product Functions

### 2.3 Operating Environment

### 2.4 Design and Implementation Constraints

### 2.5 Assumptions and Dependencies

## 3. External Interface Requirements

### 3.1 User Interfaces

### 3.2 Software Interfaces

### 3.3 Communications Interface

## 4. System Features

### 4.1 Import component

### 4.1.1 Description and Priority

The import component allows the initial information that is needed by LD Wizard to be specified by an end user.

There are two kinds of initial information that a user might provide:

  - Exactly one source data file (required; high priority).
  - At most one script file (optional; medium priority).

There are two ways in which initial information can be imported by a user:

  - Import from a local file.
  - Import from a remote URL.

### 4.1.2 Stimulus/Response Sequences

- This component must block further componets/steps in case no source file is specified.

### 4.1.3 Functional Requirements

- Preferred file extensions.
- Drag & drop (low priority).

Core requirements:
  - The ability to import exactly one data source file.
  - The ability to import exactly one script file.
  - The ability to import from a local file:
  - The ability to import from a publically accessible online location (URL).

Additional requirements:
  - TBD: Specify a soft limit for the file size:
    - There may be a limit to the file size that can be held in browser memory.
    - There may be a limit to the file size that can be submitted within one HTTP request without receiving a timeout signal from the server.
  - TBD: Automatically recognize the file format:
    - Not at all: the function signature determines how the file will be processed.
    - Based on file name: `.csv` for data imports; `.cow`, `.rml`, or `.rq` for script imports.
    - Based on a (partial) parse of the file.

Limiting scope:
  - Importing from non-SSL URLs (i.e., HTTP rather than HTTPS) is not supported.
  - Importing from SSL URLs on servers that do not emit the correct headers (e.g., CORS) is not supported.
  - It is not possible to import multiple source files or multiple script files.
  - Only CSV source data is supported.
  - File decompression is not supported.

```
import-data(URL)
import-data(file)
import-script(URL)
import-script(file)
```

### 4.2 wizard GUI component

### 4.2.1 Description and Priority

### 4.2.2 Stimulus/Response Sequences

### 4.2.3 Functional Requirements

### 4.3 Export component

Leesbaarheid: TriG (*), JSON-LD

### 4.3.1 Description and Priority

The export component allows the results of a LD Wizard transformation to be stored in simple text files.  The text files are formatted in such a way that they allow direct reuse in more advanced Linked Data transformation tools.

### 4.3.2 Stimulus/Response Sequences

### 4.3.3 Functional Requirements

Potential export formats for scripts:

  - [CoW](https://github.com/clariah/cow/wiki).
  - [RMLeditor](https://rml.io/tools/rmleditor/)
  - RATT (RDF All The Things)
  - SPARQL CONSTRUCT (for RDF-to-RDF conversions)

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
