component -> module?

<img src="/docs/img/LDWizard.png" align="right">

# LD Wizard: create Linked Data in one Spell

LD Wizard is a generic framework for converting tabular data sources to Linked Data.

# Software Requirements and Design Document (Preliminary)

## 1. Introduction

Linked Data is an increasingly more popular paradigm for publishing open data.  This is especially true in the [cultural heritage](https://www.netwerkdigitaalerfgoed.nl/tag/linked-open-data/) and [social and economic history](https://stories.datalegend.net) domains.  The publication of Linked Data usually requires extensive knowledge of Linked Data principles and technologies.  This means that only a relatively small group of technology enthusiasts has been able to publish Linked Data up till now.  At the same time, there is a much wider group of domain experts and data owners that wants to experiment with Linked Data for the first time.  This group of users is currently scarred away by the many technical hurdles that are imposed by traditional Linked Data publication strategies.

This is where **LD Wizard** comes in.  LD Wizard is a framework for creating end-user focused Graphical User Interfaces (GUIs) that simplify the creation and publication of Linked Data.  LD Wizard allows domain experts and data owners to publish standards-compliant Linked Datasets, without having to worry about Linked Data-specific intricacies.

While a perfect tool for creating simple Linked Data publications, LD Wizard also allows transformations to be exported in various script formats.  These scripts can be used in more advanced tools in order to achieve more complex Linked Data transformations.

### 1.1 Purpose

The purpose of LD Wizard is to transform simple tabular data files into standards-compliant Linked Data. LD Wizard specifically focuses on the beginning user: it does not presuppose any prior knowledge of existing vocabularies or Linked Data technologies.

At the same time, LD Wizard allows transformations to be exported into several widely used data transformation languages. These exports can be used in more advanced Linked Data transformation tools in order to expand upon the initial transformation created with LD Wizard.

### 1.2 Document Conventions

`code`: Code of any kind will be added in the document between two "\`"-marks. For multi-line code this document uses "\`\`\`"-marks to start and stop a code-block.

### 1.3 Product Scope

We distinguish between the generic **LD Wizard Interface** and various **LD Wizard Applications**.  Each LD Wizard Application is a specific implementation of the LD Wizard Interface.

The scope of the project is to create two working LDWizard tools, a hello-world LDWizard tool, and the cultural heritage LDWizard. The hello-world LDWizard will serve as basic testing tool for implementation of the advanced tooling needed for the second LDWizard. The hello-world LDWizard will also serve as a starting point for creating more specialized tooling for a specific domain. The hello-world LDWizard will be the product of the second milestone. The second LDWizard will be designed according to the specifications of the domain expert in the cultural heritage expert and will serve as a tool to transform excel sheets from the cultural heritage sector to Linked data. The cultural heritage LDWizard will be the product for the third milestone. The Software requirements as written in this document, are written for an uninstantiated LDWizard, unless specified otherwise.

### 1.4 References

| Abbreviation | Description |
|:------------:|:-----------:|
| **Base IRI** | The IRI that is used to transform relative IRIs into absolute IRIs. |
| **Cell** | The string value at the intersection of one [row]() and one [column]() in a [table]().  Cells are separated by a separator token.  This is called a *field* in [RFC 4180](https://tools.ietf.org/html/rfc4180). |
| **Column** | A vertical sequence of [cells]() in a [table](). |
| **CSV** | Comma-Separated Values: a standardize and non-proprietary tabular data format (see IETF [RFC 4180](https://tools.ietf.org/html/rfc4180)). |
| **ETL** | Extract-Transform-Load: a generic approach for creating Linked Data out of other source data formats (in this case: tabular source data). |
| **Header** | If present, the first [row]() in a [table]() that describes the meanings of values that appear in the cells that appear in the corresponding [column](). |
| **Row** | A horizontal sequence of [cells]() in a [table]().  Rows are separated by an end-of-line separator.  This is called a *record* in [RFC 4180](https://tools.ietf.org/html/rfc4180). |
| **Sheet** | A simple [spreadsheet]() that encodes one [table](). |
| **Spreadsheet** | A wrapper format around one or more [tables]().  A spreadsheet that contains multiples tables is sometimes called a [workbook](). |
| **Table** | A 2D text format that is structured in [columns]() (horizontally) and [rows]() (vertically). |
| **Workbook** | A complex [spreadsheet]() that consists of one or more [sheets](). |

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

We distinguish between the generic LD Wizard Interface and various LD Wizard Applications. Each LD Wizard Application is a specific implementation of the LD Wizard Interface.

#### LD Wizard Interface

The generic specification of functionalities that must be implemented, resulting in a specific LD Wizard Application.

The goal of LD Wizard is to provide an interface that can be implemented for a specific domain.

Secondly the framework will be designed to be customizable and expandable. Such that developers and users can customize the framework to fit their domains.

(see [Chapter 3](#ch3) and [Chapter 4](#ch4))

#### LD Wizard Applications

The following LD Wizard Applications are included in this repository. They serve as example implementations of the LD Wizard Interface:

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

Since LD Wizard Applications are client-side web applications that runs in regular and up-to-date web browsers, there are limits to the amount of data that can processed.

### 2.5 Assumptions and Dependencies

#### 2.5.1 Application assumptions

LD Wizard currently assumes that every row of the tabular source data represents exactly one entity/thing in the transformed Linked Data output.

#### 2.5.2 User assumptions

We make the following assumptions regarding these users:

- An LD Wizard developer must have a general knowledge about JavaScript and TypeScript.
- An LD Wizard developer may need some knowledge of Linked Data (TBD).

## 3. External Interface Requirements

This section specifies how LD Wizard communicates with external components.

### 3.1 User Interfaces

The User Interface requirements are specified in Chapter 4. They specify in more detail the possible steps and actions a user can take per step in the process of the LD Wizard.

The general interface is shown in [Figure 3](#GeneralUserInterface).  The inner rectangle is where specific interfaces for sub-steps are positioned.  The benefit of a generic interface is that it provides continuity in LD Wizard use when moving between the sub-steps.  The generic interface also includes consistent branding of the LD Wizard (bottom-right) and of the specific LD Wizard Application (top-left).  Links to documentation and the LD Wizard project are also included in the bottom-right corner.

<figure id="GeneralUserInterface">
  <img src="/docs/img/GeneralUserInterface.svg" width="70%" height="50%">
  <figcaption>
    Figure 3 ― Minimalistic generic user interface.
  </figcaption>
</figure>

The general User Interface will be designed as a flexible and easily updatable configurable system to create multiple different instantiated LD Wizard Applications from a single framework.

For the implementation of the interface the product will rely on [Font Awesome](https://fontawesome.com), [Material-UI](https://material-ui.com), [Recoil](https://recoiljs.org), [React](https://reactjs.org).

Steps:
  1. import
  2. configuration
  3. export
  4. publish

### 3.2 Communications Interface

The first type of communication will happen between the interface and the local file system. This type of communication will happen via buttons in the product. These buttons will open the file system folder structure. The user can select a file to upload to the LD Wizard, or when downloading the user can select a folder where the LD Wizard will store the files to.
The second line of communication that will happen from inside the project to outside the project is the download of CSV into the product.
The third case of communication that happens, is between the product and the platform on which the data will be published. To establish this connection to a data platform from the product, the product might need extra information and possible authorizations tokens. These tokens need to be stored in the product itself or have a specialized field to fill in the authorization tokens.

## 4. System Features

LD Wizard consists of four basic components as shown in [Figure 4](#FlowDiagramforLDWizard):

<dl>
  <dt>Import component</dt>
  <dd>Used for uploading the tabular source data.</dd>
  <dt>Transformation Wizard</dt>
  <dd>The main GUI component that allows users to specify a transformation to standards-compliant Linked Data.</dd>
  <dt>Export component</dt>
  <dd>Allows Linked Data and/or the transformation script to be exported to the local file system.</dd>
  <dt>Publish component</dt>
  <dd>Allows the Linked Data, tabular source data, and transformation script to be published in an online triple store.</dd>
</dl>

<figure id="FlowDiagramforLDWizard">
  <img src="/docs/img/FlowDiagramforLDWizard.svg">
  <figcaption>
    Figure 4 ― Flow chart of the LD Wizard process.  The process is subdivided into four components.
  </figcaption>
</figure>

### 4.1 Import component

The import component ([Figure 5](#ImportComponent)) is the LD Wizard component that is used for uploading the tabular source data.

<figure id="ImportComponent">
  <img src="/docs/img/ImportComponent.svg" width="70%" height="50%">
  <figcaption>
    Figure 5 ― Flow chart overview of the LD Wizard import component process.
  </figcaption>
</figure>

### 4.1.1 Description and Priority

The *import component* allows the initial information that is needed by LD Wizard to be specified by an end-user.

There are two kinds of initial information that a user might provide:

1. Exactly one tabular source data file (required; high priority).
2. At most one script file (optional; medium priority).

There are two ways in which this initial information can be imported by a user:

1. Import from a local file.
2. Import from a remote URL.

#### 4.1.1.a Tabular source data formats

In order to keep things simple, tabular source data is expected to be available in a CSV format (see [Section 4.1.1.b](#csv-upload)).  At the same time, there are many other formats for storing tabular source data.  Specifically, more advanced tabular formats like [Office Open XML Workbook](#https://en.wikipedia.org/wiki/Office_Open_XML) and [OpenDocument Spreadsheet](#https://en.wikipedia.org/wiki/OpenDocument_technical_specification) are popular in the wider user group that LD Wizard seeks to address.  For this reason LD Wizard supports the following list of tabular source format:

- Office Open XML Workbook
- OpenDocument Spreadsheet
- TBD

The above formats are converted to CSV immediately after upload.  The conversion from these tabular formats to CSV is best-effort, specifically:

- *Workbooks* with multiple sheets are not supported.  Only the first sheet is used.
- Complex visual layouts are not supported.  For example, spreadsheets with nested columns are not supported.
- Complex visual markup is not preserved.  For example, colors and fonts are not preserved, neither are bold and italic text markup.

An error is communicated to the user in case the conversion from the user-supplied tabular source format to CSV fails.

#### 4.1.1.b CSV formats

LD Wizard assumes that tabular source data is available in a CSV format.  See [Section "Tabular source data formats"](#411a-tabular-source-data-formats) for how all valid user-supplied tabular source data is guaranteed to be CSV.

While there is a standardized CSV format ([RFC 4180](https://tools.ietf.org/html/rfc4180)), there are many non-standardized CSV formats that are in wide use.

**TBD: Which CSV format(s) will we support?**

#### 4.1.1.c CSV header

We do not expect that a CSV will always have a header line.  If the file does not have a header file we should use a base IRI + the letter of the column as the IRI for the predicate.

#### 4.1.1.d CSV special characters

LD Wizard will follow the [TriG](https://www.w3.org/TR/trig) specification for handling special characters. The LD Wizard will handle these special characters as errors.

##### Limitations

The second decision we should take is the size of the CSV documents. Here two factors can be limiting for us in how large the size of the file can be. The performance of the conversion script, and the size of the document that can be handled in the browser, without significant performance loss.

To make sure we can handle both limits I would recommend using a file limit of 50 MB. If we notice that we can improve or enlarge one or both we could always improve it.

A final hard limitation would be the amount of columns, and a limit on the amount of rows. Let's set the limit for the amount of columns on 30, for now. As it is expected that this would not improve the usability of the LDWizard if we enlarge this number any further. But we can always decide different.
Let's set the amount of rows on 1.048.576, the same limit as excel for the amount of rows. With the same footnote as for the amount of columns.

**Priority: High**

### 4.1.2 Stimulus/Response Sequences

- This component must block further components/steps in case no valid source file is specified.

Stimulus: the user uploads a correct CSV document. <br>
Response: The continue/transform button will enable and the document will be stored in the browser memory.

Stimulus: The user uploads a correct CSV document but the CSV document is too large.<br>
Response: The user will get an error saying that the document is large.

Stimulus: The user wants to upload a CSV via URL, but the URL is not available.<br>
Response: The user will get an error saying that LD Wizard failed to retrieve the data.

Stimulus: The user uploads an incorrect document.<br>
Response: The user will get an error saying that the document is incorrect and the LD Wizard will show the location of the error.

Stimulus: The user uploads a correct CSV document, with incorrect special characters according to the <https://www.w3.org/TR/trig/> specifications.<br>
Response: The user will get an error saying that the document is incorrect and show the location of the error.

Stimulus: The user uploads multiple CSV documents.<br>
Response: The user will get an error saying it can only upload a single CSV document.

Stimulus: The user uploads a correct conversion script.<br>
Response: The script will be handled accordingly. The user will see a transform instead of a continue button.

Stimulus: The user uploads an incorrect script.<br>
Response: The user will get a warning that the script is incorrect.

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
  - Based on file name: `.CSV` for data imports.
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

### 4.2 LDWizard GUI component

The LDWizard GUI component and interfaces. This component builds the GUI that the general user will interact with to convert their CSV file into a linked data file.

<figure id="GUIComponent">
  <img src="/docs/img/GUIComponent.svg" width="70%" height="50%">
  <figcaption>
    Figure 6 ― LDWizard GUI component.
  </figcaption>
</figure>

### 4.2.1 Description and Priority

The configuration of the GUI is based on a number of smaller components that together create an GUI with which the user can interact to convert it's CSV to RDF. The GUI interface has two distinct groups of interfaces, ones interfaces that interact with the entire CSV document and interfaces that only interact with a single column.

#### Setting a baseIRI

General configuration setting the baseIRI for the document. The baseIRI can be used to generate IRI's from datapoints in the CSV.

#### Setting a Prefix

General configuration setting the prefixes for the document. The prefixes can be used to generate IRI's from datapoints in the CSV.

#### Setting a vocabulary

The user or the developer can add vocabularies, either linked data vocabularies or datalists, to supplement auto-complete functionality and cleaning functions. the added data helps the user to give suggestions based on the added vocabularies.

#### Setting a subject column

The user can set a column to be the subject of that row. The subject should be able to be transformed into a correct IRI. The user can also choose to not select a subject column. Then the row number will be taken as a subject column.

#### Setting a class/type for the subject column

The user can set a class for the subject column. The subject class is an IRI and can either be found with the help of autosuggest from the vocabularies, or be filled in by the user.

#### Setting a predicate term for each column

The user can set a predicate for each of the other non subject columns. The predicate is an IRI and can either be found with the help of autosuggest from the vocabularies, or be filled in by the user.

#### Setting a datatype for a column

The user can set a predicate for each of the other non subject columns. The predicate is an IRI and can either be found with the help of autosuggest from the list of standard added in vocabularies, added in vocabularies, or be filled in by the user. If no

#### Cleaning values in a column

The user is able to create a function or template which the conversion script can use to format/clean a column following a certain description.<!--  Here we need to be more specific -->

**Priority: High**

### 4.2.2 Stimulus/Response Sequences

- This section should block next sections if the ETL-conversion script is not finished.

#### Setting a baseIRI

Stimulus: The user sets an correct baseIRI<br>
Response: The baseIRI is stored in the ETL-configuration and will be applied to all selected columns

Stimulus: The user sets an incorrect baseIRI<br>
Response: The baseIRI is validated and an error is returned to the user to set a correct baseIRI.

#### Setting a prefix

Stimulus: The user sets an correct prefix<br>
Response: The prefix is stored in the ETL-configuration and will be applied to all selected columns

Stimulus: The user sets an incorrect prefix<br>
Response: The prefix is validated and an error is returned to the user to set a correct prefix.

#### Setting a vocabulary

Stimulus: The developer sets a correct vocabulary to complement the CSV.<br>
Response: The vocabulary link is stored in the ETL-configuration and can be used for cleaning/configuring object terms and setting predicate terms.

Stimulus: The developer sets multiple correct vocabularies to complement the CSV.<br>
Response: All vocabulary links are stored in the ETL-configuration and can be used for cleaning/configuring object terms and setting predicate terms.

Stimulus: The developer sets one or multiple incorrect vocabularies to complement the CSV.<br>
Response: The vocabularies can not be retrieved from their respective locations. The user will not be able to use the vocabularies, but will not notice any errors of missing vocabularies.

#### Setting a subject column

Stimulus: The user sets an allowed column as a key/subject column.<br>
Response: The subject column is stored in the ETL-configuration.

Stimulus: The user does not set an key/subject column.<br>
Response: The subject is now generated based on the rownumber and the baseIRI.

Stimulus: The user removes the key/subject column selection.<br>
Response: The user is shown a warning that it should set a subject column. The subject column is removed from the ETL-configuration and the subject is now generated based on the rownumber and the baseIRI.

Stimulus: The user wants to set a different column as a key/subject column.<br>
Response: The user is shown a warning that the subject column will be changed to the new column.

Stimulus: The user sets a different column as a key/subject column.<br>
Response: The old subject column is removed from the ETL-configuration and the new column is added as subject column to the ETL-configuration.

#### Setting a class/type for the subject column

Stimulus: The user sets an allowed subject type.<br>
Response: The subject type is stored in the ETL-configuration.

Stimulus: The user removes the subject type.<br>
Response: The user is shown a warning that it should set a subject type. The subject type is removed from the ETL-configuration.

Stimulus: The user removes the class/type column selection.<br>
Response: The subject type is removed from the ETL-configuration.

#### Setting a predicate for a column

Stimulus: The user sets a predicate for a column.<br>
Response: The predicate is stored in the ETL-configuration.

Stimulus: The user does not set a predicate for a column.<br>
Response: The predicate is now generated based on the column header name and the baseIRI.

Stimulus: The user removes the predicate term column selection.<br>
Response: The predicate type is removed from the ETL-configuration and the predicate is now generated based on the column header name and the baseIRI.

#### Setting a datatype for a column

Stimulus: The user sets a datatype for a column.<br>
Response: The datatype is stored in the ETL-configuration.

Stimulus: The user does not set a datatype for a column.
Response: If the column is not set to contain IRI's, The datatype `xsd:string` is stored in the ETL-configuration. Else no datatype is set.

Stimulus: The user removes the cleaning function for a column .<br>
Response: The old datatype is removed in the ETL-configuration and the datatype `xsd:string` is stored in the ETL-configuration.

#### Cleaning values in a column

Stimulus: The user sets a cleaning function for a column.<br>
Response: The cleaning function is stored in the ETL-configuration.

Stimulus: The user does not set a cleaning function for a column.<br>
Response: do nothing.

Stimulus: The user removes the cleaning function for a column .<br>
Response: The cleaning function is removed in the ETL-configuration.

### 4.2.3 Functional Requirements

Core requirements:

- The ability to set a baseIRI. (M)
- The ability to set one or more vocabularies to search in.
- The ability to select a subject column. (M)
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

- All core requirements, that are (M)andatory are at a minimum required to have a working LDWizard.

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

The export component of the LDWizard. This component describes all the export features.

<figure id="exportComponent">
  <img src="/docs/img/exportComponent.svg" width="70%" height="50%">
  <figcaption>
    Figure 7 ― LDWizard Export component.
  </figcaption>
</figure>

### 4.3.1 Description and Priority

#### Export transformation output

The transformed CSV data is made available for download in TriG. The LDWizard will export TriG as this format is better readable when opened. First time users will likely open their transformed files and harder to read formats such as N-Quads and N-Triples will harder to understand. TriG is able to include the graph component, so TriG is able to return complete RDF. Initially we will not allow the graph component to be set in LD Wizard, as this is normally thought of as an expert feature.

#### Export transformation script

Due to the limitations of the LDWizard as a client-side application, the ETL script inside the browser is limited to a max set of rows and columns. To use the transformation script outside of the LDWizard an export component will be made available.
The export component allows the results of an LD Wizard transformation to be stored in simple text files. The text files are formatted in such a way that they allow direct reuse in more advanced Linked Data transformation tools.

- To use the script the user designed in the LDWizard outside of the LDWizard.
- To improve/change and understand the transformation steps of the LDWizard.
- To import to the ETL-script for a different CSV in the LDWizard.

The LDWizard will be able to export the transformation script into different languages. The LDWizard will make it possible to export the ETL-script into ([RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt), [RMLeditor](https://rml.io/tools/rmleditor/) or [CoW](https://github.com/clariah/cow/wiki)) language. The default exportation language will be [RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt)

**Priority: High**

### 4.3.2 Stimulus/Response Sequences

Stimulus: The user selects an export transformation script language.<br>
Response: The user the transformation script can now be exported in the selected language.

Stimulus: The user sends a request to the export transformation script.<br>
Response: The user will receive a window to specify the location to where the transformation script is stored. The transformation script is stored in the selected language (default [RATT](https://www.npmjs.com/package/@triply/ratt)).

Stimulus: The user sends a request to the export transformation output.<br>
Response: The user will receive a window to specify the location to where the transformation output is stored.

Stimulus: The user sends a request to the export source file.<br>
Response: The user will receive a window to specify the location to where the source file is stored.

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
export-sourceFile(location)
export-transformationScript(location)
export-transformationOutput(location)
set-transformationOutput(language)
```

### 4.4 Upload/publish component

<figure id="PublishComponent">
  <img src="/docs/img/PublishComponent.svg" width="70%" height="50%">
  <figcaption>
    Figure 8 ― LDWizard publish component.
  </figcaption>
</figure>

### 4.4.1 Description and Priority

**Priority: Medium**

### 4.4.2 Stimulus/Response Sequences

### 4.4.3 Functional Requirements

### 4.5 ETL conversion script

The LDWizard will use the predefined ETL-script RATT to perform the transformation step. [RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt) is picked as the tool can be used in a client-based setting to transform CSV into RDF. [RATT](https://www.npmjs.com/package/@triply/ratt) also gives the LDWizard an expressive and expandable toolkit to create complex transformation procedures if necessary.

### 4.5.1 Description and Priority

The LDWizard will make a few assumptions about the CSV format.

- We assume that there is only one subject in the script/CSV/template
- We assume that the description about the subject in the script is handled as high as possible in the template.

The steps below are guidelines to transform a CSV file to an RDF file.

Step 1: Find the subject in the row, either the rownumber or the predefined column, set as subject.<br>
Step 2: If needed convert the subject to a proper IRI.<br>
Step 3: Move from left to right to the column, starting from the first/second depending on the location of the subject.<br>
Step 4: Skip the column if the column is not mentioned in RATT, RML, or set to skip in COW.<br>
Step 5a: Clean the value in the column we do want to parse, for now with template based cleaning.<br>
Step 5b: Set the datatype of the column we do want to parse. If the object is an IRI, make sure that we set it correctly.<br>
Step 5c: (Set/Parse column as predicate) and link the subject and the object together with the correct predicate.<br>
Step 6: Move back to step 3, until it the end of the table is reached.<br>

The conversion from RATT to RML and from RML to RATT, as also from RATT to COW and from COW to RATT should be deterministic. Thus when you download a RML script for example and then reupload the RML script is should create the exact same RATT script from the RML script, as from which the RML script was created.

With this way of stepping through the columns and conversion, we can have a better guarantee that the transformation between the 3 languages can be successful if all three languages follow these steps.<br>

For a particular csv: [csv](/docs/conversionScripts/example-1.csv)</br>
The hello-world-LDWizard is expected to create the following conversionscripts:</br>
[RATT](https://www.npmjs.com/package/@triply/ratt): [RATT](/docs/conversionScripts/example-1-RATT.ts)</br>
[CoW](https://github.com/clariah/cow/wiki): [CoW](/docs/conversionScripts/example-1.csv-metadata.json)</br>
[RMLeditor](https://rml.io/tools/rmleditor/): [RML](/docs/conversionScripts/example-1-RML.ttl)</br>

**Priority: Medium**

### 4.5.2 Stimulus/Response Sequences

Stimulus: The user uploads a correct RATT script.<br>
Response: The script gets loaded into the LDWizard.

Stimulus: The user uploads a correct RML script. <br>
Response: The script gets converted to a correctly working RATT script and loaded into the LDWizard.

Stimulus: The user uploads a correct COW script. <br>
Response: The script gets converted to a correctly working RATT script and loaded into the LDWizard.

Stimulus: The user uploads an incorrect RATT script. <br>
Response: The user gets an error, that the script is incorrect.

Stimulus: The user uploads an incorrect RML script. <br>
Response: The LDWizard tries to convert the script. But the user gets a warning, stating that the script is incorrect.

Stimulus: The user uploads an incorrect COW script. <br>
Response: The LDWizard tries to convert the script. But the user gets a warning, stating that the script is incorrect.

### 4.5.3 Functional Requirements

Core requirements:

- The ability to transform a RATT script into a RML script.
- The ability to transform a RATT script into a COW script.
- The ability to transform a RML script into a RATT script.
- The ability to transform a COW script into a RATT script.

Additional requirements:

Limiting scope:

- The transformation will transform the RATT script to a single script file in a different language.
- The transformation to a working RATT script is only guaranteed if other script file was also generated by LDWizard.
- It is not possible to tranform multiple script files.
- Only `.cow`, `.rml`, `.ts` source scripts are supported.
- File decompression is not supported.

Conversion from string to IRI

```
"1" => http://example.org/character/1
```

```
# RATT
app.use(middleware.convertToNamedNode("id", "http://example.org/character/"));

# Cow
"aboutUrl": "http://example.org/character/{id}"

# RML
rr:template "http://example.org/character/{id}"
```

Set subject type

```
http://example.org/character/1 rdf:type http://schema.org/Person
```

```
# RATT
app.use(middleware.addQuad("id",prefixes.rdf("type"),prefixes.schema("Person")));

# Cow
{
 "@id": "https://iisg.amsterdam/example-1.csv/column/id",
 "name": "id",
 "propertyUrl": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type",
 "valueUrl": "http://schema.org/Person"
}

# RML
:TriplesMap rr:predicateObjectMap [
  rr:predicate rdf:type;
  rr:objectMap [
   rr:constant schema:Person
 ]
].
```

Set predicate

```
http://example.org/character/1 http://schema.org/givenName <>
```

```
# RATT
app.use(middleware.addQuad("id", prefixes.schema("givenName"), "firstname"));
# Cow
{
  "@id": "http://schema.org/givenName",
  "name": "firstname",
  "propertyUrl": "http://schema.org/givenName"
},

# RML
:TriplesMap rr:predicateObjectMap [
  rr:predicate schema:givenName;
  rr:objectMap [
    rml:reference "firstname"
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

The product should protect any sensitive information from being uploaded/accessed outside of the product, when the user has not given explicit confirmation to do so.

### 5.4 User Documentation

For this product we will need to types of documentation. User documentation for an instantiated product and a second developers documentation for an uninstantiated product.

<!-- ### 5.5 Software Quality Attributes -->

<!-- ## 6. Other Requirements -->
