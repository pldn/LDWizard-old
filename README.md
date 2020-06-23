<img src="/docs/img/LDWizard.png" align="right">

# LDWizard

A generic framework for creating Linked Data in one Spell.

# Software Requirements and Design Document (Preliminary)

## 1. Introduction

The advent of Linked Data is evident through the ever increasing activities within [cultural heritage](https://www.netwerkdigitaalerfgoed.nl/tag/linked-open-data/) and [social and economic history](https://stories.datalegend.net). However, for most part these initiatives still rely on expert technical knowledge regarding vocabularies and data-transformation. Confronted with the question, 'so how can we do that (Linked Data)?', when presenting the results from the [2019 Hack-a-LOD](https://hackalod.com/index.php/2019/12/24/teams-en-resultaten-2019/) to the commmunity in a local drugstore, Erwin Folmer sparked the idea for LDWizard. A limited, but useful first gauge at transposing data to Linked Data for non-experts, yet with the ability for experts to advance on the output by the LDWizard.

### 1.1 Purpose

The purpose of LD Wizard is to transpose a CSV input file in a simple yet meaningful way in Linked Open Data. LD Wizard specifically forcusses on establishing an initial transformation that requires no prior knowledge of Linked Data technologies to establish. At the same time, LD Wizard allows this initial transformation to be exported into a widely used data transformation language. This export can be used with more advanced Linked Data transformation tools in order to expand upon the initial transformation created with LD Wizard.

### 1.2 Document Conventions

`code`: Code of any kind will be added in the document between two "\`"-marks. For multi line code this document uses "\`\`\`"-marks to start and stop a code-block.

### 1.3 Product Scope

The scope of the project is to create a two working LDWizard tools, a hello-world LDWizard tool, and the cultural heritage LDWizard. The hello-world LDWizard will serve as basic testing tool for implementation of the advanced tooling needed for the second LDWizard. The hello-world LDWizard will also serve as starting point for creating more specialized tooling for a specific domain. The hello-world LDWizard will be the product of the second milestone. The second LDWizard will be designed according to the specifications of the domain expert in the cultural heritage expert and will serve as a tool to transform excel sheets from the cultural heritage sector to Linked data. The cultural heritage LDWizard will be the product for the third milestone. The Software requirements as written in this document, are written for an uninstantiated LDWizard, unless specified otherwise.

### 1.4 References

| Abbreviation |                                                                                                                                                        |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| csv          | Comma-separated values, a tabular format, used as non-proprietary format for tabular data. described in [RFC4180](https://tools.ietf.org/html/rfc4180) |
| ETL          | An abbreviation for Extract data, Transform data, Load data. Which is used to describe pipelines that transform data from one type to another type.    |
| baseIRI      | An IRI that forms the basis link, which can be expanded with an extra path-element that will point to a specific resource                              |

## 2.Overall Description

The LDWizard is designed to be a starting framework for transformations from csv to linked data. The LDwizard is designed to be the starting point of a generic pipeline that can be customized and expanded to cater specified needs in designated fields.

### 2.1 Product Perspective

The goal of this product is to have a framework that can handle most of the basic transformations needed to transform csv into good linked data. Secondly the framework will be designed to be customizable and expandable. Such that developers and users can customize the framework to fit their domains.

### 2.2 Product Functions

With the hello-world LDWizard, a developer is able to create an instantiated LDWizard based on the functions and interfaces that are available to the developer. Each of the functions/Interface are described and defined in chapter 3 and 4.

With the instantiated LDWizard, the user is able to import a csv. With the imported csv the user will get an overview of the data and can create an step by step conversion schema to convert the csv to a TriG linked data file.

### 2.3 Operating Environment

The product will operate inside one of the major browser and will be designed to work as a client-side application only. The application should work independent of the operating software, but it is expected that the product will only work in the newer browsers.

- Operating system: All
- Browsers: Firefox, Chrome, Chromium, Edge.
- platform: Typescript/csv/RML/JSON

### 2.4 Design and Implementation Constraints

The LDWizard will be a clientside, browser only framework. This means that the framework will have limits based on the hardware and the software the users have installed and which browsers the users currently have.
To make sure the LDWizard can still perform. We will assume that the user will use the recent versions of the browsers available on their machine.

### 2.5 Assumptions and Dependencies

The LDWizard assumes that there are three different types of users of the LDWizard.

- A general user, somebody who will use an instantiated product set up by developers and domain experts.
- A Linked data specialist, somebody who will use an instantiated product set up by the developers to kick start the development of a ETL.
- A developers and domain experts, people who will use the LDWizard framework to set up their own instantiated version of the LDWizard. Catered for their domain/specific usecase.

The LDWizard assumes that all three types of users have a general knowledge of linked data. We also assume that the developer will have a general knowledge about Javascript and/or Typescript.

The LDWizard will at this moment assume that per row only one subject is allowed.

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

### 4.1.1 Description and Priority

The import component allows the initial information that is needed by LD Wizard to be specified by an end user.

There are two kinds of initial information that a user might provide:

- Exactly one source data file (required; high priority).
- At most one script file (optional; medium priority).

There are two ways in which initial information can be imported by a user:

- Import from a local file.
- Import from a remote URL.

#### csv upload

For the csv upload we will need to make a choice about how we interpret a correct csv document. This is due to the ambiguity of a correct csv file. A csv file can have multiple different delimiter formats, e.g. "," or ";". This could occur natural if the user uses the Dutch notation for decimal numbers. When this happens, the csv split will differ from what is expected.

Secondly a csv can have multiple different methods for declaring strings with quotations marks, e.g. """ or "'". Finally there are also different Implementations for spaces at the beginning and the end of the fields. These can also be handled different from csv to csv.

For this problem there are three solutions, we can either declare that:

- A correct csv document, is something that the developers from LDWizard decide.
- A correct csv document, is something the implementer of an instantiated LDwizard will decide.
- A correct csv document, is something the user of a specific instantiated LDWizard will decide on a limited basis.

<!-- I would recommend that we implement the second solution as leading. The domain expert that will help create the instantiation of the LDWizard will probably also know which csv template is leading the domain. The domain expert can also help if users have the incorrect csv, and help them transform the csv file. -->

We do not expect that a csv will always have a header line. If the file does not have a header file we should use a baseIRI + the letter of the column as the IRI for the predicate.

The LDWizard will follow the <https://www.w3.org/TR/trig/> specifications for the handling of special characters. The LDWizard will handle these special characters as errors.

##### Limitations

The second decision we should take is the size of the csv documents. Here two factors can be limiting for us in how large the size of the file can be. The performance of the conversion script, and the size of the document that can be handled in the browser, without significant performance loss.

To make sure we can handle both limits I would recommend using a file limit of 50 mb. If we notice that we can improve or enlarge one or both we could always improve it.

A final hard limitation would be the amount of columns, and a limit on the amount of rows. Let's set the limit for the amount of columns on 30, for now. As it is expected that this would not improve the usability of the LDWizard if we enlarge this number any further. But we can always decide different.
Let's set the amount of rows on 1.048.576, the same limit as excel for the amount of rows. With the same footnote as for the amount of columns.

### 4.1.1 Description and Priority

### 4.1.2 Stimulus/Response Sequences

- This component must block further components/steps in case no source file is specified.

Stimulus: the user uploads a correct csv document. <br>
Response: The continue/transform button will enable and the document will be stored in the browser memory.

Stimulus: The user uploads a correct csv document but the csv document is too large.
Response: The user will get a error saying that the document is large.

Stimulus: The user wants to upload a csv via URL, but the URL not available.
Response: The user will get a error saying that LDWizard failed to retrieve the data.

Stimulus: The user uploads an incorrect document.<br>
Response: The user will get a error saying that the document is incorrect and the LDWizard will show the location of the error.

Stimulus: The user uploads an correct csv document, with incorrect special characters according to the <https://www.w3.org/TR/trig/> specifications.<br>
Response: The user will get a error saying that the document is incorrect and show the location of the error.

Stimulus: The user uploads multiple csv documents.<br>
Response: The user will get a error saying it can only upload a single csv document.

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

Leesbaarheid: TriG (\*), JSON-LD

### 4.3.1 Description and Priority

The export component allows the results of a LD Wizard transformation to be stored in simple text files. The text files are formatted in such a way that they allow direct reuse in more advanced Linked Data transformation tools.

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

### 4.5 ETL script && conversion bindings

Instead of developing a new ETL-tool we will use an existing ETL-tool to execute the transformation step. We decided to use a client based transformation tool [RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt) as the tool to transform the csv to RDF. The LDWizard uses this language due to it's expandability and ease of use.

Due to the limitations of the LDWizard, the ETL script inside the browser is limited to a max set of rows and columns. To use the ETL-script on a larger csv the LDWizard has an export button to export the ETL-script. This has two advantages.

- To use the script the user designed in the LDWizard outside of the LDWizard.
- To import to the ETL-script for a different csv in the LDWizard.

To make the use of the ETL-script more generic we will give the user the possibility to export the ETL-script into different languages. At the moment LDWizard will make it possible to export the ETL-script into ([RATT (RDF All The Things)](https://www.npmjs.com/package/@triply/ratt), [RMLeditor](https://rml.io/tools/rmleditor/) or [CoW](https://github.com/clariah/cow/wiki)) language.

### 4.5.1 Description and Priority

The conversion from RATT to RML and from RML to RATT, as also from RATT to COW and from COW to RATT should be deterministic. Thus when you download a RML script for example and then reupload the RML script is should create the exact same RATT script from the RML script, as from which the RML script was created.

The following guidelines for the transform between RATT and COW/RML and between COW/RML and RATT are recommended.(These can be expanded upon when new information arises)

To keep it simpler for now I will make a few assumptions about the data for now. But some/all of these restrictions could be removed in a later point of the process.

- We assume that there is only one subject in the script/csv/template
- We assume that the description about the subject in the script is handled as high as possible in the template.

Step 1: Find the subject in the row, either the rownumber or the predefined column, set as subject.<br>
Step 2: If needed convert the subject to a proper IRI.<br>
Step 3: Move from left to right to the column, starting from the first/second depending on the location of the subject.<br>
Step 4: Skip the column if the column is not mentioned in RATT, RML, or set to skip in COW.<br>
Step 5a: Clean the value in the column we do want to parse, for now with template based cleaning.<br>
Step 5b: Set the datatype of the column we do want to parse. If the object is an IRI, make sure that we set it correctly.<br>
Step 5c: (Set/Parse column as predicate) and link the subject and the object together with the correct predicate.<br>
Step 6: Move back to step 3, until it the end of the table is reached.<br>

With this way of stepping through the columns and conversion, we can have a better guarantee that the transformation between the 3 languages can be successful if all three languages follow these steps.<br>

<!-- When supporting multiple subjects per row we need to expand the steps 1,3 and 4. As then we need to move through the columns and skip not only based on if a column is used, but also if the column is used w.r.t the choosen subject.   -->

### 4.5.2 Stimulus/Response Sequences

Stimulus: The user uploads a correct RATT script .<br>
Response: The script gets loaded into the LDWizard.

Stimulus: The user uploads a correct RML script. <br>
Response: The script gets converted to a correctly working RATT script and loaded into the LDWizard.

Stimulus: The user uploads a correct COW script. <br>
Response: The script gets converted to a correctly working RATT script and loaded into the LDWizard.

Stimulus: The user uploads an incorrect RATT script. <br>
Response: The user gets a error, that the script is incorrect.

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

## 5. Other (Non)functional Requirements

Each of the requirements below are requirements important to note, but do not belong to an interface, or an functional component.

### 5.1 Performance Requirements

There are no explicit performance requirements. The performance of the application should feel smooth while clicking through the steps. When the conversion process is running let's give the user then feedback on how the process is doing.

### 5.2 Safety Requirements

The app is a clientside only app. This will limited the amount of safety requirements needed for the software application stack.

### 5.3 Security Requirements

The product should protect any sensitive information from being uploaded/accessed outside of the product, when the user has not given explicit confirmation to do so. All the

<!-- ### 5.4 Software Quality Attributes -->

### 5.4 User Documentation

For this product we will need to types of documentation. An user documentation for an instantiated product and a second developers documentation for an uninstantiated product.

<!-- ## 6. Other Requirements -->
