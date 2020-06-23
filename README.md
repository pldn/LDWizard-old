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

This section provides information to ensure that the system will communicate properly with external components.

### 3.1 User Interfaces

The specific user interface requirements are written in chapter 4 and describe in more detail the possible steps and actions a user can take per step in the process of the LDWizard. The general interface as shown in [Figure 1](#GeneralUserInterface), is designed with a specific interface for each step inside of a general interface outside of the specific interfaces, with buttons to move between the Sections in the LDWizard, ow with the section buttons on the top. The LDWizard logo is shown in the top right corner, and in the bottom left corner the logo of the instance hosting the LDWizard and important links can be shown, configurable as well.

<figure id="GeneralUserInterface">
  <img src="/docs/img/GeneralUserInterface.svg" width="70%" height="50%">
  <figcaption>
    Figure 1 ― Minimalistic generic user interface.
  </figcaption>
</figure>

The general user interface will be designed as a flexible and easily updatable configurable system to create multiple different instantiated LDWizards from a single framework.

For the implementation of the interface the product will rely on fontawesome, material-ui, recoil, react.

### 3.2 Communications Interface

The first type of communication will happen between the interface and the local file system. This type of communication will happen via buttons in the product. These buttons will open the file system folder structure. The user can select a file to upload to the LDWizard, or when downloading the user can select an folder where the LDWizard will store the files to.
The second line of communication that will happen from inside the project to outside the project is the download of csv into the product.
The third case of communication that happens, is between the product and the platform on which the data will be published. To establish this connection to a dataplatform from the product, the product might need extra information and possible authorizations tokens. These tokens need to be stored in the product itself or have a specialized field to fill in the authorization tokens.

## 4. System Features

The basic LDWizard consists out of 4 basic components as shown in [Figure 2](#FlowDiagramforLDWizard):

- The upload/input component, for uploading the to be transformed csv and a possible transformation script.
- Wizard GUI component, GUI components that will handle one or multiple transformation processes.
- Download/export component, For downloading/exporting the linked data and transformation script to your local file system.
- Upload/publish component, For uploading/publishing the linked data and transformation script on the web.

<figure id="FlowDiagramforLDWizard">
  <img src="/docs/img/FlowDiagramforLDWizard.svg">
  <figcaption>
    Figure 2 ― Flow chart for the LDWizard, dividing the 4 basic components for the LDWizard
  </figcaption>
</figure>

### 4.1 upload/input component

Software component for uploading files to the LDWizard or inputting files to the LDWizard.

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

**Priority: High**

### 4.1.2 Stimulus/Response Sequences

- This component must block further components/steps in case no source file is specified.

Stimulus: the user uploads a correct csv document. <br>
Response: The continue/transform button will enable and the document will be stored in the browser memory.

Stimulus: The user uploads a correct csv document but the csv document is too large.<br>
Response: The user will get a error saying that the document is large.

Stimulus: The user wants to upload a csv via URL, but the URL not available.<br>
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

### 4.2 LDWizard GUI component

The configure compontent of the LDWizard. This component describes all the steps that can be taken to create the transformation from csv to RDF.

### 4.2.1 Description and Priority

The configure step consists out of, a general configuration, and a column specific part. In the general configure step the user or a developer can set configurations that have an effect on complete document. For the column specific part we can were for each column the user can tweak a number of configurations on a column per column basis.

#### Setting a baseIRI

General configuration setting the baseIRI for the document. The baseIRI can be used to generate IRI's from datapoints in the csv.

#### Setting a Prefix

General configuration setting the prefixes for the document. The prefixes can be used to generate IRI's from datapoints in the csv.

#### Setting a vocabulary

The user or the developer can add vocabularies, either linked data vocabularies or datalists, to supplement auto-complete functionality and cleaning functions. the added data helps the the user to give suggestions based on the added vocabularies.

#### Setting a subject column

The user can set a column to be the subject of that row. The subject should be able to be transformed into a correct IRI. The user can also choose to not select a subject column. Then the rownumber will be taken as a subject column.

#### Setting a class/type for the subject column

The user can set a class for the subject column. The subject class is an IRI and can either be found with the help of autosuggest from the vocabularies, or be filled in by the user.

#### Setting a predicate term for each column

The user can set a predicate for each of the other non subject columns. The predicate is an IRI and can either be found with the help of autosuggest from the vocabularies, or be filled in by the user.

#### Setting a datatype for a column

The user can set a predicate for each of the other non subject columns. The predicate is an IRI and can either be found with the help of autosuggest from the list of standard added in vocabularies, added in vocabularies, or be filled in by the user. If no

#### Cleaning values in a column

The user is able to create a function or template which the conversion script can use to format/clean a column following a certain description.<!--  Here we need to be more specific -->

#### Mark the object term for column as IRI

The user is able to mark the object term of a column as IRI. The object term will now be handled as an IRI and wont be needing a datatype/language.

#### Skipping a column

The user is able to skip a column, notifying the ETL-conversion that this column should not be taken in account in the conversion script.

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

Stimulus: The developer sets a correct vocabulary to complement the csv.<br>
Response: The vocabulary link is stored in the ETL-configuration and can be used for cleaning/configuring object terms and setting predicate terms.

Stimulus: The developer sets multiple correct vocabularies to complement the csv.<br>
Response: All vocabulary links are stored in the ETL-configuration and can be used for cleaning/configuring object terms and setting predicate terms.

Stimulus: The developer sets one or multiple incorrect vocabularies to complement the csv.<br>
Response: The vocabularies can not be retrieved from their respective locations. The user will not be able to use the vocabularies, but will not notice no errors of missing vocabularies.

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
Response: If the column is not set to contains IRI's, The datatype `xsd:string` is stored in the ETL-configuration. Else no datatype is set.

Stimulus: The user removes the cleaning function for a column .<br>
Response: The old datatype is removed in the ETL-configuration and the datatype `xsd:string` is stored in the ETL-configuration.

#### Setting term for column as IRI.

Stimulus: The user marks the column as IRI.<br>
Response: The IRI configuration is stored in the ETL-configuration.

Stimulus: The user does mark the column as IRI.<br>
Response: do nothing.

Stimulus: The user removes the mark as IRI from the column.<br>
Response: The IRI configuration is removed from the ETL-configuration.

#### Cleaning values in a column

Stimulus: The user sets a cleaning function for a column.<br>
Response: The cleaning function is stored in the ETL-configuration.

Stimulus: The user does not set a cleaning function for a column.<br>
Response: do nothing.

Stimulus: The user removes the cleaning function for a column .<br>
Response: The cleaning function is removed in the ETL-configuration.

#### Skipping a column

Stimulus: The user checks the skip flag for a column.<br>
Response: The skip flag is stored in the ETL-configuration.

Stimulus: The user does not set the skip flag for a column.<br>
Response: do nothing.

Stimulus: The user removes the skip flag from column selection.<br>
Response: The skip flag is removed in the ETL-configuration.

### 4.2.3 Functional Requirements

Core requirements:

- The ability to set a baseIRI. (M)
- The ability to set one or more vocabularies to search in.
- The ability to select a subject column. (M)
- The ability to set an class for a subject.
- The ability to set a predicate for each column. (M)
- The ability to skip a column.
- The ability to mark the object term for column as IRI.
- The ability to clean the values in a column for each column.
- The ability to set a datatype for the values in a column for each column. (M)

Additional requirements:

- For all of the mandatory core requirements an basic solution is required.
  - Use the URL of the instance, account, and datasetName (The ability to set a baseIRI).
  - Use the rownumber to create the IRI (The ability to select a subject).
  - Use the columnheader names to to create the predicate terms (The ability to set a predicate for each column).
  - Set `xsd:string` as datatype for all object terms(The ability to set a datatype for the values in a column for each column).

Limiting scope:

- All core requirements, that are (M)andatory are at a minimum required to have a working LDWizard.

```
set-baseIRI(baseIRI)
set-availableVocabulary(URL)
set-subject(column)
set-class(IRI)
set-predicate(column,IRI)
set-cleaningOperation(function|template)
set-datatype(datatype)
```

### 4.3 Export component

Leesbaarheid: TriG (\*), JSON-LD

### 4.3.1 Description and Priority

The export component allows the results of a LD Wizard transformation to be stored in simple text files. The text files are formatted in such a way that they allow direct reuse in more advanced Linked Data transformation tools.

**Priority: High**

### 4.3.2 Stimulus/Response Sequences

### 4.3.3 Functional Requirements

Potential export formats for scripts:

- [CoW](https://github.com/clariah/cow/wiki).
- [RMLeditor](https://rml.io/tools/rmleditor/)
- RATT (RDF All The Things)
- SPARQL CONSTRUCT (for RDF-to-RDF conversions)

### 4.4 Upload/publish component

### 4.4.1 Description and Priority

**Priority: Medium**

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

**Priority: Medium**

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

The app is a client-side only app. This will limited the amount of safety requirements needed for the software application stack.

### 5.3 Security Requirements

The product should protect any sensitive information from being uploaded/accessed outside of the product, when the user has not given explicit confirmation to do so.

<!-- ### 5.4 Software Quality Attributes -->

### 5.4 User Documentation

For this product we will need to types of documentation. An user documentation for an instantiated product and a second developers documentation for an uninstantiated product.

<!-- ## 6. Other Requirements -->
