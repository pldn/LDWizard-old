<img src="/docs/img/LDWizard.png" align="right">

# LD Wizard: create Linked Data in one Spell

LD Wizard is a framework for creating end-user focused Graphical User Interfaces (GUIs) that simplify the creation and publication of Linked Data.
<!-- LD Wizard is a generic framework for converting tabular data sources to Linked Data. -->

# Software Requirements and Design Document (Preliminary)

## 1. Introduction

Linked Data is an increasingly more popular paradigm for publishing open data. This is especially true in the [cultural heritage](https://www.netwerkdigitaalerfgoed.nl/tag/linked-open-data/) and [social and economic history](https://stories.datalegend.net) domains. The publication of Linked Data usually requires extensive knowledge of Linked Data principles and technologies. This means that only a relatively small group of technology enthusiasts has been able to publish Linked Data up till now. At the same time, there is a much wider group of domain experts and data owners that want to experiment with Linked Data for the first time. This group of users is currently scarred away by the many technical hurdles that are imposed by traditional Linked Data publication strategies.

This is where **LD Wizard** comes in. LD Wizard is a framework for creating end-user focused Graphical User Interfaces (GUIs) that simplify the creation and publication of Linked Data. LD Wizard allows domain experts and data owners to publish standards-compliant Linked Datasets, without having to worry about Linked Data-specific intricacies.

While a perfect tool for creating simple Linked Data publications, LD Wizard also allows the transformation-configuration to be exported in various script formats. These scripts can be used in more advanced tools in order to achieve more complex Linked Data transformations.

### 1.1 Purpose

The purpose of LD Wizard is to transform simple tabular data files into standards-compliant Linked Data. LD Wizard specifically focuses on the beginning user: it does not presuppose any prior knowledge of existing vocabularies or Linked Data technologies.

At the same time, LD Wizard allows transformations to be exported into several widely used data transformation languages. These exports can be used in more advanced Linked Data transformation tools in order to expand upon the initial transformation created with LD Wizard.

### 1.2 Document Conventions

`code`: Code of any kind will be added in the document between two "\`"-marks. For multi-line code this document uses "\`\`\`"-marks to start and stop a code-block.

### 1.3 Product and Project Scope

We distinguish between the generic **LD Wizard Interface** and various **LD Wizard Applications**. Each LD Wizard Application is a specific implementation of the LD Wizard Interface.

#### 1.3.1 Project Scope

The scope of the project is to create the LD Wizard Interface and two working LD Wizard Applications, a hello-world LD Wizard Application, and the cultural heritage LD Wizard Application. The hello-world LD Wizard will serve as basic testing tool for implementation of the advanced tooling needed for the second LD Wizard. The hello-world LD Wizard will also serve as a starting point for creating your own LD Wizard Application for a specific domain. The hello-world LD Wizard will be the product of the second milestone. The second LD Wizard will be designed according to the specifications of the domain expert in the cultural heritage expert and will serve as a tool to transform excel sheets from the cultural heritage sector to Linked Data. The cultural heritage LD Wizard will be the product for the third milestone. The Software requirements as written in this document, are written for an uninstantiated LD Wizard, unless specified otherwise.

### 1.4 Terminology

|  Abbreviation   |                                                                                                  Description                                                                                                  |
| :-------------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|  **Base IRI**   |                                                                      The IRI that is used to transform relative IRIs into absolute IRIs.                                                                      |
|    **Cell**     | The string value at the intersection of one [row]() and one [column]() in a [table](). Cells are separated by a separator token. This is called a _field_ in [RFC 4180](https://tools.ietf.org/html/rfc4180). |
|   **Column**    |                                                                               A vertical sequence of [cells]() in a [table]().                                                                                |
|     **CSV**     |                                   Comma-Separated Values: a standardize and non-proprietary tabular data format (see IETF [RFC 4180](https://tools.ietf.org/html/rfc4180)).                                   |
|     **ETL**     |                                   Extract-Transform-Load: a generic approach for creating Linked Data out of other source data formats (in this case: tabular source data).                                   |
|   **Header**    |                           If present, the first [row]() in a [table]() that describes the meanings of values that appear in the cells that appear in the corresponding [column]().                            |
|     **Row**     |               A horizontal sequence of [cells]() in a [table](). Rows are separated by an end-of-line separator. This is called a _record_ in [RFC 4180](https://tools.ietf.org/html/rfc4180).                |
|    **Sheet**    |                                                                             A simple [spreadsheet]() that encodes one [table]().                                                                              |
| **Spreadsheet** |                                       A wrapper format around one or more [tables](). A spreadsheet that contains multiples tables is sometimes called a [workbook]().                                        |
|    **Table**    |                                                         A 2D text format that is structured in [columns]() (horizontally) and [rows]() (vertically).                                                          |
|  **Workbook**   |                                                                      A complex [spreadsheet]() that consists of one or more [sheets]().                                                                       |

### 1.5 Attribution

LD Wizard is an initiative of the following organizations and people:

- [Dutch Digital Heritage Network (NDE)](https://www.netwerkdigitaalerfgoed.nl/en), Enno Meijers & Ivo Zandhuis
-  [The Netherlands’ Cadastre, Land Registry and Mapping Agency (Kadaster)](https://www.kadaster.nl), Erwin Folmer
- [International Institute of Social History (IISH)](https://iisg.amsterdam/en), Richard Zijdeman
- [Triply](https://triply.cc), Thomas de Groot, Gerwin Bosch & Wouter Beek

## 2.Overall Description

LD Wizard is designed to be a starting framework for transformations from CSV to Linked Data. LD Wizard is designed to be the starting point of a generic pipeline that can be customized and expanded to cater specified needs in designated fields.

### 2.1 Product Perspective

The transformation of Linked Data requires the combination of expertise in the following three roles:

LD Wizard distinguishes between the following types of users:

<dl>
  <dt>General user</dt>
  <dd>Uses an LD Wizard Application in a specific domain.</dd>
  <dt>Linked Data Expert</dt>
  <dd>Uses an LD Wizard Application to create an initial transformation.</dd>
  <dt>Developer</dt>
  <dd>Creates an LD Wizard Application by implementing the LD Wizard Interface for a specific domain or use case.</dd>
</dl>

1. Domain Expert: has knowledge about the domain that is described in the data.

2. Linked Data Expert: is able to create a semantic data model, and is able to define the structure of the source data in Linked Data.

3. Programmer: is able to implement the transformation from tabular source data to Linked Data, using a programming language.

Because the combination of expertise of these three roles in one person is quite rare, Linked Data transformation often required multiple people working together. This also means that the domain expert is dependent on the Linked Data Expert and the Programmer in order to make a change to the transformation (even for small changes).

This traditional Linked Data transformation approach is characterized
in [Figure 1](#traditional-etl).

<figure id="traditional-etl">
  <img src="/docs/img/traditional-etl.svg" width="70%" height="50%">
  <figcaption>
    Figure 1 ― Schematic overview of a traditional Linked Data ETL.
  </figcaption>
</figure>

Linked Data tools generalize work normally performed by a Programmer, so that a Domain Expert and a Linked Data Expert are able to transform Linked Data without the involvement of a Programmer. Examples of such approaches are COW and RML.

LD Wizard further separates the roles required for Linked Data transformation: it also generalizes work normally performed by a Linked Data Expert, so that a Domain Expert is able to transform Linked Data
herself.

The LD Wizard approach is depicted in [Figure 2](#ld-wizard-approach). The grey horizontal bar represents the 'happy flow' of a general user. This user is able to transform tabular source data into standards-compliant Linked Data without continuous dependencies on a Linked Data Expert or Developer.

Developers are able to create new LD Wizard Applications, to support general users in specific domains or use cases. Linked Data Expert are able to take the transformation that a general user has created, allowing them to extend it using more advanced transformer tools (i.e., outside LD Wizard).

<figure id="ld-wizard-approach">
  <img src="/docs/img/ld-wizard-approach.svg" width="70%" height="50%">
  <figcaption>
    Figure 2 ― Schematic overview of the LD Wizard approach.  The grey horizontal bar represents the 'happy flow' of a general user.
  </figcaption>
</figure>

### 2.2 Product Functions

We distinguish between the generic LD Wizard Interface and various LD Wizard Applications. Each LD Wizard Application is a specific implementation of the generic LD Wizard Interface.

#### 2.2.1 LD Wizard Interface

The generic specification of functionalities that must be implemented.  The LD Wizard Interface is generic, and as such is not yet optimized for one particular domain or for one particular use case.  An implementation of the LD Wizard Interface results in a specific LD Wizard Application.  The latter can be specifically optimized for a particular domain and/or for a particular use case.

The LD Wizard Interface is designed to be customizable and expandable.  A developer who satisfies the assumptions specified in [Section 2.5.2](#252-user-assumptions) must be able to create an LD Wizard Application.

#### 2.2.2 LD Wizard Applications

The following LD Wizard Applications are part of this repository. They serve as concrete examples of how the generic LD Wizard Interface can be made concrete:

<dl>
  <dt>Hello World Wizard</dt>
  <dd>A minimal configuration that implements the LW Wizard Interface.  This wizard is fully functional, but does not serve a particular domain.  It is intended to be used by developers who want to configure their own wizard, since it shows how interfaces can be implemented in practice.  The Hello World Wizard is also an ideal basis for a new, domain-specific configuration.</dd>
  <dt>NDE Wizard</dt>
  <dd>A configuration that implements the LD Wizard Interface for the cultural heritage domain.  This wizard is fully functional and serves a particular domain.  It is intended to be used by domain experts from the cultural heritage domain who want to transform their tabular source data into standards-compliant Linked Data.</dd>
</dl>

### 2.3 Operating Environment

The product will operate inside one of the major browsers and will be designed to work as a client-side application only. The application should work independent of the operating software, but it is expected that the product will only work in the newer browsers.

- Operating System: any
- Web browser: recent versions of Firefox, Chrome, Edge, and Safari.
- Technology: TypeScript, JavaScript, HTML, CSS

### 2.4 Implementation Constraints

Since LD Wizard Applications are client-side web applications that run in regular and up-to-date web browsers, there are limits to the amount of data that can processed.

### 2.5 Assumptions and Dependencies

This section specified the assumptions that we make regarding LD Wizard components and LD Wizard users.  These assumptions set forth the constraints within which the LD Wizard is able to optimally operate.

#### 2.5.1 Source data assumptions

We make the following assumptions regarding the source data:

- The tabular source data file must have a header row.
- Every row of the tabular source data is assumed to represent exactly one thing.  In other words, every row the source data describes one person, location, creative work, etc.
- All rows in the tabular source data are assumed to represent things of the same type.  For example, if some rows in the tabular source data describe locations and some rows in the tabular source data describe persons, this file must first be split into two separate files that are transported separately.  Notice that in general it is unlikely that different types of things (in this
- Source data that is uploaded to a publication platform is assumed to have an open license.
- The tabular source data is assumed to have at most 30 columns.
- The tabular source data is assumed to have at most 1,048,576 rows.

#### 2.5.2 User assumptions

We make the following assumptions regarding the three user groups.

Assumptions for general users:

- General users are assumed to be able to export their tabular source data to CSV.

Assumptions for developers:

- Must have a general knowledge about JavaScript and TypeScript.
- May need some knowledge of Linked Data (TBD).

#### 2.5.3 Publication platform assumptions

Assumptions for publication platforms:

- The publication platform must be able to process RDF data in the [TriG]() encoding format.

## 3. External Interface Requirements

This section specifies how LD Wizard communicates with external components.

### 3.1 User Interfaces

While LD Wizard developers are able to customize the appearance of their LD Wizard Applications, the LD Wizard Interface does specify some generic properties of the LD Wizard user interface.  This is done for the following reasons:

1. It ensures that all LD Wizard Applications look and work similar from the perspective of the generic user.  Specifically, a user who has interacted with one LD Wizard Application should feel comfortable to use another LD Wizard Application, because the generic appearance and interaction pattern are the same.
2. It simplifies the job of the developer.  She can focus on customizing the domain-specific parts while reusing the generic parts.

The generic interface is shown in [Figure 3](#GenericUserInterface).  The inner rectangle is where specific interfaces for specific interaction steps are located.  The goal of the generic interface twofold:

1. Solidify the branding of LD Wizard (bottom-right corner) and allow custom branding on a per-application basis (top-left corner).
2. Provide the overall interaction flow between the various interaction steps.

The benefit of a generic interface is that it provides continuity for generic users, when they move between the various interaction steps.  Links to documentation and the LD Wizard project are also included in the branding component that is positioned in the bottom-right corner.

<figure id="GenericUserInterface">
  <img src="/docs/img/GeneralUserInterface.svg" width="70%" height="50%">
  <figcaption>
    Figure 3 ― Schematic overview of the generic LD Wizard user interface.
  </figcaption>
</figure>

For the implementation of the interface, LD Wizard will make use of the following commonly used and well-maintained web libraries:

- [Font Awesome](https://fontawesome.com)
- [Material-UI](https://material-ui.com)
- [React](https://reactjs.org).
- [Recoil](https://recoiljs.org)

The following interaction steps are located within the inner rectangle:

1. import
2. configuration
3. export
4. publish

### 3.2 Communications Interface

Three types of communication are expected between the LD Wizard and other applications.

- Communications between the LD Wizard and the local file system to retrieve the tabular data sources, for transformation.  The generic user will activate the communications to the file system via the user interface.
- Communications between the LD Wizard and the local file system to store the Linked Data result file, the transformation script, and the original tabular source data file.  The end-user will activate the communications to the file system via the user interface.
- Communications between the LD Wizard and a external Linked Data publication platform, in order to store the Linked Data result, the transformation script, and the tabular source data.  Publication on such a platform typically requires authorization and authentication.  The LD Wizard Interface must be extendable to facilitate interaction with linked data publication platforms.  For the purpose of authorization and authentication, the generic user must supply additional information.  Additional information consist of a user name and password combination, or it may consist of an API token that the generic user has created for the intended linked data publication platform.

## 4. System Features

LD Wizard consists of four basic components as shown in [Figure 4](#FlowDiagramforLD Wizard):

<dl>
  <dt>Import component</dt>
  <dd>The component that is used to upload tabular source data.</dd>
  <dt>Transformation component</dt>
  <dd>The component that is used to specify the data transformation from the tabular source format to a standards-compliant Linked Data.</dd>
  <dt>Export component</dt>
  <dd>The component that allows the transformed Linked Data, the transformation script, and the tabular source data to be downloaded to the local file system.</dd>
  <dt>Publish component</dt>
  <dd>The component that allows the transformed Linked Data, the transformation script, and the tabular source data to be published in an online Linked Data environment.</dd>
</dl>

<figure id="FlowDiagramforLD Wizard">
  <img src="/docs/img/FlowDiagramforLDWizard.svg">
  <figcaption>
    Figure 4 ― Schematic overview of the overall LD Wizard process.  The process is subdivided into sub-processes that correspond to the four LD Wizard components.
  </figcaption>
</figure>

The following subsections specify the four LD Wizard components in more detail.

### 4.1 Import component

The import component ([Figure 5](#ImportComponent)) allows a generic user to provide the initial information that is needed in order to start a data transformation.

<figure id="ImportComponent">
  <img src="/docs/img/ImportComponent.svg" width="70%" height="50%">
  <figcaption>
    Figure 5 ― Schematic overview of the LD Wizard import process.
  </figcaption>
</figure>

### 4.1.1 Description and Priority

The import component allows the initial information that is needed by an LD Wizard Application to be specified by a generic user.  There are two kinds of initial information that a generic user might provide:

1. Exactly one tabular source data file (high priority).
2. At most one transformation script file (low priority).

There are two ways in which this initial information can be provided by a user:

1. Import from a local file (high priority).
2. Import from a remote HTTPS URL (low priority).

#### 4.1.1.a Tabular source data formats

In order to keep things simple, tabular source data is expected to be available in a CSV format (see [Section 4.1.1.b](#411b-csv-formats)).  At the same time, there are many other formats for storing tabular source data.  Specifically, more advanced tabular formats like [Office Open XML Workbook](#https://en.wikipedia.org/wiki/Office_Open_XML) and [OpenDocument Spreadsheet](#https://en.wikipedia.org/wiki/OpenDocument_technical_specification) are popular in the wider user group that LD Wizard seeks to address.

Most spreadsheet applications have the ability to export to CSV.  Such conversions generally yield standard-compliant CSV.  Because CSV is a relatively simple format, not all aspects of the tabular source format are preserved, specifically:

- Workbooks with multiple sheets are not supported.  Only the first sheet is used.
- Complex visual layouts are not supported.  For example, nested columns are not supported.
- Complex visual markup is not preserved.  For example, colors and fonts are not preserved, neither are bold and italic text markup.

#### 4.1.1.b CSV formats

LD Wizard assumes that tabular source data is available in a CSV format.  Tabular source data that is not in a CSV format is discussed in [Section 4.1.1.a](#411a-tabular-source-data-formats).

The CSV format is standardized in [RFC 4180](https://tools.ietf.org/html/rfc4180) by the Internet Engineering Taskforce (IETF).  The most commonly used applications for editing tabular data (e.g., [Microsoft Excel](https://en.wikipedia.org/wiki/Microsoft_Excel), [LibreOffice Calc](https://en.wikipedia.org/wiki/LibreOffice_Calc)) are able to export tabular data to this standardized CSV format.

At the same time, some hand-crafted CSV files may deviate from the standard in the following ways:

<dl>
  <dt>Cell separator</dt>
  <dd>The CSV file may use a different token than comma (<code>,</code>) for separating the cells.   For example, the semi-colon (<code>;</code>) is popular when cells commonly contains comma's and cell values are not delimited by double quotes (<code>"</code>).  In some locates the comma is used as the decimal separator, e.g., the Dutch locale.</dd>
  <dt>Cell quoting</dt>
  <dd>Double quotes (<code>"</code>) are used to surround cell values that contain the separator token (<code>,</code> or <code>;</code>).  Double quotes that appear inside a cell value are escaped with the double quote prefix (i.e., <code>"a""b"</code> denotes a cell value of three characters).
  <dt>Row ending</dt>
  <dd>The end of a row is denoted by an end of line characters.  <a href="https://tools.ietf.org/html/rfc4180" target="_blank">RFC 4180</a> specifies <code>CRLF</code> for this, but some files use <code>CR</code> or <code>LF</code> instead.<!--_-->
  <dt>Character encoding</dt>
  <dd>While many character encodings exist and are in use, UTF-8 (which includes ASCII) is by far the most common one.  Automatic character encoding detection is relatively difficult and error-prone, so LD Wizard assumes that the CSV source data file uses UTF-8 encoding.</dd>
</dl>

The LD Wizard import component supports *all* CSV files that follow the [RFC 4180](https://tools.ietf.org/html/rfc4180) standard, and *some* CSV files that deviate from the standard.  Support for standard CSV files is guaranteed, while support for non-standard CSV files is best effort.

#### 4.1.1.c CSV header

LD Wizard assumes that the first row of the CSV source data file encodes the header row.  The header row is assumed to define what each column is about.

#### 4.1.1.d CSV file size & dimensions

LD Wizard places limits on the size and dimensions of the CSV source data that are supported.

Firstly, the maximum (byte)size of the CSV source data file is determined by the following two factors:
- the maximum file size supported by modern web browsers
- the performance of the conversion script

To stay well within these limits, LW Wizard supports CSV source files of at most 50 MB (uncompressed) in size.

Secondly, LD Wizard sets a limit to the number of rows and columns that it supports:
- the maximum number of columns is 30.
- the maximum number of rows is 1,048,576.

### 4.1.2 Stimulus/Response Sequences

This section specifies the sequence of user actions that results in a transformation to Linked Data.  Earlier steps in this sequence block later steps to maintain an orderly flow.

1. *The user imports a correct CSV file.*
   The continue/transform button will be enabled and the tabular source file will be stored in the web browser memory.
2. *The user imports a CSV file that is too large.*
   The user receives an error stating that the file exceeds the maximum supported file size.
3. *The user imports a CSV file from a remote HTTPS URL, but the resource denoted by that URL is not available*
   The user receives an error stating that the remote file could not be retrieved.
4. *The user imports a syntactically incorrect CSV file*
   The user receives an error message stating that the file is incorrect.  The error message includes an overview of the part of the source data file that caused the error.
5. *The user imports more than one CSV file.*
   The user receives an error message stating that only one tabular source file can be imported.
6. *The user imports a correct conversion script.*
   The script is handled accordingly.  The user will see a transform instead of a continue button.
7. *The user imports an incorrect script.*
   The user receives an error message stating that the script is incorrect.

### 4.1.3 Functional Requirements

Core requirements:

- The ability to import exactly one data source file.
- The ability to import at most one transformation script.
- The ability to import from a local file.
- The ability to import from a publicly accessible online location (URL).

Additional requirements:

- Specify a soft limit for the file size:
  - There may be a limit to the file size that can be held in browser memory.
  - There may be a limit to the file size that can be submitted within one HTTP request without receiving a timeout signal from the server.
- Automatically recognize the file format:
  - Not at all: the function signature determines how the file will be processed.
  - Based on file name: `.csv` for data imports.
  - Based on a (partial) parse of the file.

Limiting scope:

- Importing from non-SSL URLs (i.e., HTTP rather than HTTPS) is not supported.
- Importing from SSL URLs on servers that do not emit the correct headers (e.g., CORS) is not supported.
- It is not possible to import multiple source files.
- Only CSV source data is supported.
- Compressed files are not supported.

### 4.2 LD Wizard configuration component

This section specifies the LD Wizard transformation component and its interfaces.  This component presents the Graphical User Interface (GUI) that the general user will interact with after they have successfully uploaded a tabular source file.

<figure id="GUIComponent">
  <img src="/docs/img/GUIComponent.svg" width="70%" height="50%">
  <figcaption>
    Figure 6 ― Schematic overview of the LD Wizard GUI component.
  </figcaption>
</figure>

### 4.2.1 Description and Priority

The configuration is composed of a number of smaller sub-components that together compose the full transformation configuration.  We can distinguish between the different types of transformation sub-components, based on their transformation scope:

<dl>
  <dt>Table scope</dt>
  <dd>Transformations that modify (aspects of) the entire table.  For example, setting the base IRI impacts all rows, columns, and possibly cells.</dd>
  <dt>Column scope</dt>
  <dd>Transformations that modify the property that corresponds to a column, and transformations that modify every cell within the same column.</dd>
  <dt>Row scope</dt>
  <dd>Transformations that modify the subject term that corresponds to a row.</dd>
  <dt>Cell scope</dt>
  <dd>Transformation that modify individual cell values.</dd>
</dl>

Notice that configuration sub-component types become increasingly more complex to configure by a general user, as they progress from table to cell scope.  For example, there is only one input table and there are at most 30 columns, but there can be over one million rows and there can be millions of cells.  Because LD Wizard focuses on simplicity, it mainly focuses on configuration sub-components of table and column scope (high priority).  Configuration sub-components of row and cell scope have low priority.

#### 4.2.1.a Setting a base IRI

A table scope configuration that determines the string prefix for all RDF subject terms and non-mapped RDF predicate terms.

The base IRI must be a valid absolute IRI.

The developer is able to configure a default base IRI.  This IRI will be used if the generic user does not specify one.

#### 4.2.1.b Setting a vocabulary prefix

General configuration setting the prefixes for the document. The prefixes can be used to generate IRI's from data points in the CSV.

The vocabulary prefix must be a valid absolute IRI.

The developer is able to configure a default vocabulary prefix.  This vocabulary prefix will be used if the user does not specify one.

#### 4.2.1.c Setting a key column

A table scope configuration that determines the column whose cell values will be used to compose RDF subject terms.

RDF subject terms are composed by concatenating the base IRI with a normalized version of the value in the key column cell.  Normalization is needed to ensure that all subject terms are valid IRIs.

If the values that appear in the key column are not unique, a dialog with the following options is presented to the user:

<dl>
  <dt>Modify</dt>
  <dd>The subject IRIs are suffixed with `-1`, `-2`, etc. to force them to be unique.
  <dt>Continue</dt>
  <dd>The subject IRIs are not modified.  Information in two or more rows will be merged into one RDF record.</dd>
  <dt>Cancel</dt>
  <dd>Undoes the selection of the key column.  The user must choose a different key column or must use the default option (i.e., the respective row numbers).</dd>
</dl>

If the user does not specify a key column, the row number will be used.  The first row receives row number 1.  Row numbers are guaranteed to be unique.

#### 4.2.1.d Setting a class

A table scope configuration that determines the class that every RDF subject term will be an instance of.

The developer is able to pre-configure this component to provide class suggestions from a specific vocabulary or domain.

If the user does not specify a class, then `rdfs:Resource` will be used.

#### 4.2.1.e Setting a predicate term

A column scope configuration that allows one RDF predicate term to be selected for each column.

The developer is able to pre-configure this component to provide predicate suggestions form a specific vocabulary or domain.

Ideally, the header label of each column can be used to provide an initial suggestion for the generic user.

If a column was chosen as the key column (Section [TODO](#todo)), then this column cannot be configured in this configuration.

If the generic user does not specify a predicate for a column, a new IRI will be composed out of the base IRI followed by a normalized version of the header label.  Normalization is needed to ensure that all predicate terms are valid IRIs.

The user is able to skip columns on a per-column basis (e.g., a skip option could be part of the predicate selection element).

If the predicate IRIs are not unique, a dialog with the following options is presented to the user:

<dl>
  <dt>Modify</dt>
  <dd>The predicate IRIs are suffixed with <code>-1</code>, <code>-2</code>, etc. to force them to be unique.
  <dt>Continue</dt>
  <dd>The predicate IRIs are not modified.  Values from two or more rows will be exported for the same subject/predicate combination.</dd>
</dl>

#### 4.2.1.f Setting a datatype IRI

A column scope configuration that allows one datatype IRI to be specified for each column.  The selected datatype IRI will be used for each cell value in that column.

Values that appear in the selected column are assumed to be valid lexical expressions in the configured datatype IRI.  See the [XML Schema Datatypes](https://www.w3.org/TR/xmlschema11-2) standard for more information.

The developer is able to pre-configure this component to provide datatype IRI suggestions from a specific vocabulary or domain.

If the user does not specify a datatype IRI or language tag, the datatype IRI `xsd:string` is used as a default.

#### 4.2.1.g Setting a language tag

A column scope configuration that allows one language tag to be specified for each.  The selected language tag will be used for each cell value in that column.

Values that appear in the selected column are assumed to be valid lexical expressions for datatype IRI `rdf:langString`.  See the [RDF 1.1 Concepts and Abstract Syntax](https://www.w3.org/TR/2014/REC-rdf11-concepts-20140225) standard for more information.

If the user does not specify a datatype IRI or language tag, the datatype IRI `xsd:string` is used as a default.

#### 4.2.1.h Creating IRIs for cells

TBD

#### 4.2.1.i Skip empty cells

A table scope configuration that allows empty cells to be excluded from the RDF export.

#### 4.2.1.j Cleaning values in a column

A column scope configuration that allows values in individual cells to be modified.

The user is able to create a function or template which the conversion script can use to format/clean a column following a certain description.<!--  Here we need to be more specific -->

### 4.2.2 Stimulus/Response Sequences

- This section should block next sections if the ETL-conversion script is not finished.

#### 4.2.2.a Setting a base IRI

- *The user sets an correct baseIRI*
  The baseIRI is stored in the ETL-configuration and will be applied to all selected columns

- *The user sets an incorrect baseIRI*
  The baseIRI is validated and an error is returned to the user to set a correct baseIRI.

#### 4.2.2.b Setting a vocabulary prefix

- *The user sets an correct prefix*
  The prefix is stored in the ETL-configuration and will be applied to all selected columns.

- *The user sets an incorrect prefix*
  The prefix is validated and an error is returned to the user to set a correct prefix.

#### 4.2.2.c Setting a key column

- *The user selects a valid key column.*
  The key column becomes part of the configuration.

- *The user does not set an key column.*
  Subject terms will be generated based on the row number and base IRI.

- *The user removes the key column selection.*
  The key column is not longer configured.  Subject terms will be generated based on the row number and base IRI.

- *The user selects a different column as the key column.*
  The user is shown a warning that the subject column will be changed to the new column.

- *The user sets a different column as a key/subject column.*
  The old subject column is removed from the ETL-configuration and the new column is added as subject column to the ETL-configuration.

#### 4.2.2.d Setting a class

- *The user sets an allowed subject type.*
  The subject type is stored in the ETL-configuration.

- *The user removes the subject type.*
  The user is shown a warning that it should set a subject type. The subject type is removed from the ETL-configuration.

- *The user removes the class/type column selection.*
  The subject type is removed from the ETL-configuration.

#### 4.2.2.e Setting a predicate

- *The user sets a predicate IRI for a column.*
  The predicate IRI is stored in the configuration.

- *The user does not set a predicate for a column.*
  A predicate IRI is constructed, based on the vocabulary prefix and the normalized header label, and added to the configuration.

- *The user sets the 'Skip column' option.*
  The column will not be mapped to a predicate term and will not appear in the RDF export.

- *The user removes a previously set predicate.*
  The previous predicate IRI is removed from the configuration and a predicate IRI is constructed, based on the vocabulary prefix and the normalized header label, and added to the configuration.

#### 4.2.2.f Setting a datatype IRI

- *The user sets a datatype for a column.*
  The datatype IRI is stored in the configuration.

- *The user does not set a datatype IRI for a column.*
  The datatype `xsd:string` is used in the configuration.

- *The user removes a previously set datatype IRI for a column .*
  The datatype IRI `xsd:string` is used in the configuration.

#### 4.2.2.g Setting a language tag

TBD

#### 4.2.2.h Cleaning cell values

- *The user sets a cleaning function for a column.*
  The cleaning function is stored in the configuration.

- *The user does not set a cleaning function for a column.*
  do nothing.

- *The user removes the cleaning function for a column .*
  The cleaning function is removed in the configuration.

### 4.2.3 Functional Requirements

Core requirements:

- The ability to set a base IRI.
- The ability to set one or more vocabularies to search in.
- The ability to select a key column.
- The ability to set an class for a subject.
- The ability to set a predicate for each column. (M)
- The ability to clean the values in a column for each column.
- The ability to set a datatype for the values in a column for each column. (M)

Additional requirements:

- For all of the mandatory core requirements a basic solution is required, thus are required to have default behavior.
  - Use the URL of the instance, account, and datasetName (The ability to set a baseIRI).
  - Use the row number to create the IRI (The ability to select a subject).
  - Use the column header names to to create the predicate terms (The ability to set a predicate for each column).
  - Set `xsd:string` as datatype for all object terms(The ability to set a datatype for the values in a column for each column).

Limiting scope:

- All core requirements, that are (M)andatory are at a minimum required to have a working LD Wizard.

```
set-baseIRI(baseIRI)
set-Prefix(IRI)
import-vocabulary(URL)
set-subjectColumn(Column)
set-class(IRI)
set-predicate(column,IRI)
set-cleaningOperation(function|template)
set-datatype(datatype)
convert()
```

### 4.3 Export component

The export component of the LD Wizard. This component describes all the export features.

<figure id="exportComponent">
  <img src="/docs/img/exportComponent.svg" width="70%" height="50%">
  <figcaption>
    Figure 7 ― LD Wizard Export component.
  </figcaption>
</figure>

### 4.3.1 Description and Priority

#### Export transformation output

The transformed CSV data is made available for download in TriG. The LD Wizard will export TriG as this format is better readable when opened. First time users will likely open their transformed files and harder to read formats such as N-Quads and N-Triples will harder to understand. TriG is able to include the graph component, so TriG is able to return complete RDF. Initially we will not allow the graph component to be set in LD Wizard, as this is normally thought of as an expert feature.

#### Export transformation script

Due to the limitations of the LD Wizard as a client-side application, the ETL script inside the browser is limited to a max set of rows and columns. To use the transformation script outside of the LD Wizard an export component will be made available.
The export component allows the results of an LD Wizard transformation to be stored in simple text files. The text files are formatted in such a way that they allow direct reuse in more advanced Linked Data transformation tools.

- To use the script the user designed in the LD Wizard outside of the LD Wizard.
- To improve/change and understand the transformation steps of the LD Wizard.
- To import to the ETL-script for a different CSV in the LD Wizard.

The LD Wizard will be able to export the transformation script into different languages. The LD Wizard will make it possible to export the ETL-script into ([RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt), [RMLeditor](https://rml.io/tools/rmleditor/) or [CoW](https://github.com/clariah/cow/wiki)) language. The default exportation language will be [RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt)

**Priority: High**

### 4.3.2 Stimulus/Response Sequences

- *The user selects an export transformation script language.*
  The user the transformation script can now be exported in the selected language.

- *The user sends a request to the export transformation script.*
  The user will receive a window to specify the location to where the transformation script is stored. The transformation script is stored in the selected language (default [RATT](https://www.npmjs.com/package/@triply/ratt)).

- *The user sends a request to the export transformation output.*
  The user will receive a window to specify the location to where the transformation output is stored.

- *The user sends a request to the export source file.*
  The user will receive a window to specify the location to where the source file is stored.

### 4.3.3 Functional Requirements

Core requirements:

- The ability to set the transformation script language.
- The ability to export the source file.
- The ability to export the transformation output.
- The ability to export the transformation script.

Additional requirements:

- Potential export formats for scripts:
  - [CoW](https://github.com/clariah/cow/wiki).
  - [RMLeditor](https://rml.io/tools/rmleditor/)
  - RATT (RDF All The Things)

```
set-transformationOutput(language)
export-sourceFile(location)
export-transformationScript(location)
export-transformationOutput(location)
```

### 4.4 Upload/publish component

The LD Wizard Upload/publish component and interfaces. This publish component is the final component of the LD Wizard and is bridge between the LD Wizard and specialized Linked data tools.

<figure id="PublishComponent">
  <img src="/docs/img/PublishComponent.svg" width="70%" height="50%">
  <figcaption>
    Figure 8 ― LD Wizard publish component.
  </figcaption>
</figure>

### 4.4.1 Description and Priority

The publish component allows the end-user to publish their finalized linked data as domain-experts without having to worry about Linked Data-specific problems. The LD Wizard is agnostic to the tooling that reads the transformed data into their platform. he LD Wizard publish component will be able to publish the transformed linked data, the transformation script, and the source data to a platform of the users choice.

- The end-user needs specify the authorization configuration, such that the datafiles can be stored on the dataplatform.
- The end-user needs to specify the storage location, where to the datafiles will be stored.
- The end-user can upload their datafiles to the configured data platforms.

**Priority: Medium**

### 4.4.2 Stimulus/Response Sequences

- *The user selects an export transformation script language to publish the transformation script.*
  The user the transformation script can now be exported in the selected language.

- *The user configures the authorization configuration.*
  The LD Wizard now has the authorization protocols to publish the data files to the data-platform.

- *The user configures the publication location of the datafiles.*
  The LD Wizard now has the location where to publish the data files to the data-platform.

- *The user sends a request to the publish the transformation script on a data platform.*
  The LD Wizard will publish the transformation script on the data-platform the user configured. The transformation script is stored in the selected language (default [RATT](https://www.npmjs.com/package/@triply/ratt)).

- *The user sends a request to the publish the transformation output on a data platform.*
  The LD Wizard will publish the transformation output on the data-platform the user configured.

- *The user sends a request to the publish the source file on a data platform.*
  The LD Wizard will publish the source file on the data-platform the user configured.

- *The user misconfigured the authorization configuration, and tries to send a request to upload a file to a dataplatform.*
  The user will get an error message explaining why the user is not allowed to publish the data file.

### 4.4.3 Functional Requirements

Core requirements:

- The ability to set the transformation script language.
- The ability to set authorization configuration to contact the external triple store.
- The ability to set publication location to store the files on the triple store.
- The ability to publish the source file.
- The ability to publish the transformation output.
- The ability to publish the transformation script.

Additional requirements:

- Potential publish formats for scripts:
  - [CoW](https://github.com/clariah/cow/wiki).
  - [RMLeditor](https://rml.io/tools/rmleditor/)
  - RATT (RDF All The Things)

```
set-authorization(config)
set-publishLocation(config)
set-transformationOutput(language)
publish-sourceFile(location)
publish-transformationScript(location)
publish-transformationOutput(location)
```

### 4.5 ETL conversion script

The LD Wizard will use the predefined ETL-script RATT to perform the transformation step. [RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt) is picked as the tool can be used in a client-based setting to transform CSV into RDF. [RATT](https://www.npmjs.com/package/@triply/ratt) also gives the LD Wizard an expressive and expandable toolkit to create complex transformation procedures if necessary.

### 4.5.1 Description and Priority

The LD Wizard will make a few assumptions about the CSV format.

- We assume that there is only one subject in the script/CSV/template
- We assume that the description about the subject in the script is handled as high as possible in the template.

The conversion from RATT to RML and from RML to RATT, as also from RATT to COW and from COW to RATT should be deterministic. Thus when you download a RML script for example and then reupload the RML script is should create the exact same RATT script from the RML script, as from which the RML script was created.

For a particular csv: [csv](/docs/conversionScripts/example-1.csv)</br>
We expect that the following conversionscripts result in the same linked data.</br>
For [RATT](https://www.npmjs.com/package/@triply/ratt) we created the [conversion script](/docs/conversionScripts/example-1-RATT.ts) to convert the tabular data source to linked data.</br>
For [CoW](https://github.com/clariah/cow/wiki): we created the [conversion script](/docs/conversionScripts/example-1.csv-metadata.json) to convert the tabular data source to linked data.</br>
For [RMLeditor](https://rml.io/tools/rmleditor/): we created the [conversion script](/docs/conversionScripts/example-1-RML.ttl) to convert the tabular data source to linked data.</br>

**Priority: Medium**

### 4.5.2 Stimulus/Response Sequences

- *The user uploads a correct RATT script.*
  The script gets loaded into the LD Wizard.

- *The user uploads a correct RML script.*
  The script gets converted to a correctly working RATT script and loaded into the LD Wizard.

- *The user uploads a correct COW script.*
  The script gets converted to a correctly working RATT script and loaded into the LD Wizard.

- *The user uploads an incorrect RATT script.*
  The user gets an error, that the script is incorrect.

- *The user uploads an incorrect RML script.*
  The LD Wizard tries to convert the script. But the user gets a warning, stating that the script is incorrect.

- *The user uploads an incorrect COW script.*
  The LD Wizard tries to convert the script. But the user gets a warning, stating that the script is incorrect.

### 4.5.3 Functional Requirements

Core requirements:

- The ability to transform a RATT script into a RML script.
- The ability to transform a RATT script into a COW script.
- The ability to transform a RML script into a RATT script.
- The ability to transform a COW script into a RATT script.

Additional requirements:

Limiting scope:

- The transformation will transform the RATT script to a single script file in a different language.
- The transformation to a working RATT script is only guaranteed if other script file was also generated by LD Wizard.
- It is not possible to tranform multiple script files.
- Only `.cow`, `.rml`, `.ts` source scripts are supported.
- File decompression is not supported.

The following paragraphs explain how the common configuration components are translated the three different transformation languages. The first piece of code is the transformation and expected result.
The three parts below are how the three transformation languages handle the transformations. Based on the following tabular data we will transform the data into a small rdf file.

| id          | Male       |
| :--------:  | :--------: |
| 0           | 0          |

##### Expected Linked data

```ttl
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
<http://example.org/character/0> rdf:type <http://schema.org/Person> .
<http://example.org/character/0> <http://schema.org/gender> "sex-F" .
```


#### Cleaning value in a column

Conversion of a value to cleaned/standardized field. In this example the transformation goal is to transform the not understandable values in the column `Male` to an explicit representation.

expected result:
```
"sex-F"
```

###### RATT
The conversion of the RATT script uses a function that describes the `IF` statement and maps the values to an explicit stringValue.

```ts
app.use(
  middleware.cleanColumn("male", male => {
    if (male === "0") { return "sex-F"; } else { return "sex-M"; }
  })
);
```

###### CoW

The CoW implementation uses an template which maps the value of the column to the new cleaned value.

```json
{
 "@id": "https://iisg.amsterdam/example-2.csv/column/male",
 "name": "male",
 "valueUrl": "{% if male == '0' %}sex-F{% else %}sex-M{% endif %}"
},
```

###### RML

The RML implementation uses an template which maps the value of the column to the new cleaned value.

```ttl
:TriplesMap rr:predicateObjectMap [
  rr:objectMap [
    rr:template "{% if male == '0' %}sex-F{% else %}sex-M{% endif %}"
  ]
].
```

#### Creating subject column

The user sets a column to be the subject of that row. The value in this row is converted to an IRI, and acts as the subject of that row. expected result:

```ttl
<http://example.org/character/0>
```

###### RATT

RATT will use an agnostic approach and converts the ID to an IRI. In the configuration object the columnName is stored as subject identifier.

```ts
app.use(middleware.convertToNamedNode("id", "http://example.org/character/"));
```

###### CoW

 By default, the things described by each row don't have identifiers associated with them, in the csvw. To add the identifier per row, CoW can add an template to the "aboutUrl" to create an subject per row.

```json
{
  "aboutUrl": "http://example.org/character/{id}"
}
```

###### RML

RML uses an subjectMap to create the subject from csv, it is expected that each RML file has exactly one subjectMap. Else the RML structure would not be valid. The rr:subjectMap can also be structured with a template.

```ttl
:TriplesMap rr:subjectMap [
  rr:template "http://example.org/character/{id}"
].
```

#### Setting a class/type for the subject column

The user can set a class for the subject column. This will be the `rdf:type` relation between the instance of the row and a concept or class. In the example dataset it will look like:

```ttl
<http://example.org/character/0> rdf:type <http://schema.org/Person>
```

###### RATT

The addition of a class to RATT happens with the middleware.addQuad. This middleware connects the column where the subject in resides, and links it to the class.

```ts
app.use(
  middleware.addQuad("id", prefixes.rdf("type"), prefixes.schema("Person"))
);
```

###### CoW

To add a type relation with RML, a column is selected or a virtual column is created which describes the link between the subject with `valueUrl` "template", but the template points to a constant `schema:Person`.


```json
{
  "@id": "https://iisg.amsterdam/example-1.csv/column/id",
  "name": "id",
  "propertyUrl": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type",
  "valueUrl": "http://schema.org/Person"
}
```

###### RML

To add a type relation with RML, a predicateObjectMap is created which describes the link from the subject with `rr:constant` to a `schema:Person`.

```ttl
:TriplesMap rr:predicateObjectMap [
  rr:predicate rdf:type;
  rr:objectMap [
   rr:constant schema:Person
 ]
].
```

<!-- #### Setting a datatype for a column

Not only a predicate can be set, but also the datatype could be configured in the configurer. This is reflected in the ETL-scripts. The ETL-scripts can add the datatype to the following string:

```ttl
"sex-F"^^xsd:string
```

###### RATT

A function is called for each column, adding the datatype of the column values.

```ts
app.use(middleware.setDatatype("male", "xsd:string"));
```

###### CoW

A field can be added to each column, describing the datatype of the column values.

```json
{
  "@id": "https://LDWizard.com/example-1.csv/column/male",
  "name": "male",
  "datatype": "xsd:string"
},
```

###### RML

A field can be added to each column, describing the datatype of the column values.

```ttl
:TriplesMap rr:predicateObjectMap [
  rr:objectMap [
    rml:reference "male"
    rr:datatype xsd:string
  ]
].
``` -->


#### Setting a predicate for a column

The user can set a predicate for a columns of the tabular datasource. The predicate is an IRI and can either be found results either in a prefixed IRI or a expanded IRI. The resulting triple looks like:

```ttl
<http://example.org/character/0> <http://schema.org/gender> "sex-F"^^xsd:string
```

###### RATT

RATT adds the quad that has the `id` in the subject position and `male` as the object column.

```ts
app.use(middleware.addQuad("id", prefixes.schema("gender"), "male"));
```

###### CoW

To describe the predicate for the column `male` we make use of the `propertyUrl` property.

```json
{
  "@id": "https://LDWizard.com/example-1.csv/column/male",
  "name": "male",
  "propertyUrl": "http://schema.org/gender"
},
```

###### RML

The predicate for a particular column for a particular predicateObjectMap is defined with the `rr:predicate` predicate.

```ttl
:TriplesMap rr:predicateObjectMap [
  rr:predicate schema:gender;
  rr:objectMap [
    rml:reference "male"
  ]
].
```

## 5. Other (Non)functional Requirements

Each of the requirements below are requirements important to note, but do not belong to an interface, or a functional component.

### 5.1 Performance Requirements

There are no explicit performance requirements. The performance of the application should feel smooth while clicking through the steps. When the conversion process is running let's give the user then feedback on how the process is doing.

### 5.2 Safety Requirements

The app is a client-side only app. This will limit the number of safety requirements needed for the software application stack.

### 5.3 Security Requirements

The product should protect any sensitive information from being uploaded/accessed outside of the product, when the user has not given explicit confirmation to do so. The user should be able to access all the components of the LD Wizard without needing additional privileges.

### 5.4 User Documentation

For this product we will need to types of documentation. User documentation for an instantiated product and a second developers documentation for an uninstantiated product.

<!-- ### 5.5 Software Quality Attributes -->

<!-- ## 6. Other Requirements -->

*This document is based on templates introduced by:
IEEE Recommended Practice for Software Requirements Specifications," in IEEE Std 830-1998 , vol., no., pp.1-40, 20 Oct. 1998, doi: 10.1109/IEEESTD.1998.88286.*
