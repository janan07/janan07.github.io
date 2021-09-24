# Easytrieve Language Support Plugin

Easytrieve Language Support enhances the Easytrieve programming experience on your integrated Development Environment (IDE). The extension leverages the language server protocol to provide autocomplete, syntax highlighting and coloring, and diagnostic features for Easytrieve code and macros. The Easytrieve Language Support extension can also connect to a mainframe using a Zowe CLI z/OSMF profile to automatically retrieve macros used in your programs and store them in your PC.

Easytrieve Language Support recognizes files with the extension `.ezt` as Easytrieve files.

**Note:** Currently, the Easytrieve Langauage Support plug-in works only on Windows 10. For full functionality, other tools or plugins may be needed.

Other tools which can work in combination with the plug-in include the following:

- Zowe CLI
- Zowe Explorer
- JCL Language support
- DB2 (client, connect, server)
- Oracle (client, server)
- Easytrieve for Windows
 ## Table of Contents

_Question: Is Configuring and Basic Settings the same? If so, we should remove basic Settings and keep all of this under Configuring._


* Introduction
* Plug-in Features
* Installing
* Plugin Settings
  * General settings
    * Zowe profile
    * Options Table
    * Macros
    * eztsql
  * JCL Generate Settings
    * Jobcard
    * Cbaaload
    * Ezoptbl
    * Jobparm
    * Eztdsn
  * Other settings
    * Db_Name_Override
    * Env
* Configuring 
  * Configuring the plug-in
    * Option Table Setup
    * Macro Location Setup
    * Create a Zowe Profile
    * Interface Setup
  * Configuring your database
  * Configuring JCL Generate Options
  * Configuring the plugin for Windows
* Using
  * Use-case Scenarios
## Introduction
 
The Easytrieve Language Support plugin provides a subset of LSP (Language Server Protocol) functions including source code validation, the hover feature, Go To definition, Autocompletion, Options table editing, Local build-and-run task, Mainframe communication tools, Easytrieve build-and-run, and other LSP features. 

The Language Support Plugin provides a streamline way to select a wide range of basic Easytrieve Report Generator settings including the Options Table, Macros, eztsql, and Zowe Profiles. 
 
For more information about basic settings, see [Basic Settings](#basic-settings). 

There are also numerous additional settings that can be applied including JCL Generate Options as well as various other options.

For more information about additional settings, see [Additional Settings](#additional-settings).

## Plug-in features

The Easytrieve Language Support plugin provides the following subset of LSP (Language Server Protocol) functions:

* **Source code validation**

  _Add detail to definition_

  Error and warning messages from a modified Easytrieve compiler are the same as when the source code is compiled under the same conditions (options table, database connection, etc.) 

* **Hover**

  The hover functionality provides additional information to symbols (fields and filenames). Use the hover function by placing the cursor over the field name or file name. Information may be limited if the symbol is not defined in the edited file (i.e. symbol is defined in a macro).
     
* **Go to definition**

  The `Go to definition` feature makes it possible to jump to a specific location where a symbol (field or filename) is defined. This function is limited to a currently edited file. This feature is not available for symbols defined in a macro.

* **Autocompletion** (context-specific)

  Autocompletion can be used when a collon `:` is used after a filename. Enter **ctrl** + **space** to see a list of all fields defined for the file. _(Describe the limitations of this feature)_

* **Options table editing**

   _Is the error checking the only options table editing feature to be listed here?_

  - Error checking of the options table is available.

* **Local build and run task**

  When using the Windows version of Easytrieve, the code created using the plug-in can be executed on a local PC and the output can be viewed using VScode.
   
* **Mainframe communication tools**

  Mainframe communication tools make it possible to execute an Easytrieve program on the mainframe. The created Eastrieve program can be uploaded to the mainframe by using the **Upload EZT to mainframes** feature. The dataset reference is subsequently used in the generated JCL.

* **Easytrieve build-and-run**

  _Add definition_

* **Generate JCL**

  The **generate JCL** feature and Zowe explorer make it possible to executed an Easytrieve program on the mainframe.

* **Syntax colouring**

  An Easytrieve program developed using this plug-in is syntax coloured. The coloring schema uses VScode standards.

## Installing 

Installation of the plugin is via the VSCode marketplace. For more information about installation from the VS Code marketplace, see [Extension Marketplace](#https://code.visualstudio.com/docs/editor/extension-marketplace). Following installation, basic plug-in functionality is available. For full functionality, other tools or plug-ins may be required. 

_Does it make sense to list the Plugin settings here, or should we continue with Configuring? Are the Plugin Settings and Configuring synonymous?_

## Plugin Settings

### General settings

The following general settings can be customized according to user specifications:

* **zoweProfile** 

  _Why is the heading zoweProfile if there are two profiles which need to be specified (zOSMF profile and zowe profile)?_

  Specify the following profiles:
  
  * **zOSMF profile**
  
    This profile is used for mainframes execution (JCL generate, Easytrieve program upload) 

  * **zowe profile**
  
    This profile is the default profile. To use the default profile, leave this field empty.  
    
  For more information, see [Zowe Explorer Profiles](https://docs.zowe.org/stable/user-guide/ze-profiles/).

- _What is the field that should be specified for this setting?_

  Specify if you want VSCode to communicate with the mainframe (e.g. downloading macros, uploading generated JCLs) 

  
  For more information, see [Creating a zosmf profile](https://docs.zowe.org/stable/getting-started/cli-getting-started/#creating-a-zosmf-profile).

* **Options Table**

  _Are the following notes to the user who is filling in the fields? If so, can we describe the procedure?_

  - Use the path to the file containing the options table definitions in text format. Use the `.def` extension.
  - Use the same format as is used as input for `etopload`. (**_link to EZT Techdocs_**)
  - Ensure to include an empty line at the end. 
  - The default is empty. This results in a W703 warning. - **_link to EZT Techdocs_**)
  - The relative path is evaluted against first root of the workspace
  - Ensure that you update whenever other values other than default values are applied in the options table for validating and parsing the code by the language server (plugin)

    For more information, see [Create or Modify an Options Table](https://techdocs.broadcom.com/us/en/ca-mainframe-software/devops/ca-easytrieve-report-generator/11-6/using/workbench/configuration-manager/create-or-modify-an-options-table.html) in the Easytrieve documentation.

* **Macros**

  _Is this what is presented in the UI, or is this a direction to the user who is filling in the Macros field? If so, can we describe this as a procedure?_

  - List of directories (absolute path) or DSN with macros seperated by semicolon
  - default: empty
  - An absolute path is treated as a local directory
  - Other values other thatn absolute paths are  treated as a dataset name. To download macros from datasets click **refresh macros** in the status bar. Alternatively, you can execute the following line in the command pallete:
  
    `EZT refresh macros`

  - Specify the list locations of the macros, which are used in the Easytrieve code.
  
    For more information, see macro directories in the Easytrieve documentation. (**_Add link_**)

    **_Please specify which link is suitable:_**
    * [Macro Statement](https://techdocs.broadcom.com/us/en/ca-mainframe-software/devops/ca-easytrieve-report-generator/11-6/language-reference/statements/macro-statement.html)
    * [Define Macros](https://techdocs.broadcom.com/us/en/ca-mainframe-software/devops/ca-easytrieve-report-generator/11-6/programming/macro-facility/define-macros.html)

* **eztsql** 

  **_Is this a direction to the user? If so, can we describe this as a procedure?_**

  **_Is this the same as Setting Environment Variables?_**

  - The same as EZTSQL environment variable in Easytrieve itself (**_link to EZT Techdocs_**)
  - This value controls the type of database connection (EZTSQL)
  - The default database connection is ODBC.
  - Possible values for this field are the following databases:
    * ODBC
    * DB2
    * ORACLE 
  - After changing this value, it is necessary to  restart the language server by restarting VSCode.  
  - Specify if your scripts are using database. This specification ensures that the database connection is used during validating and parsing of the Easytrive code.

* **JCL generate settings**

  The JCL generate feature of the plugin enables the user to create JCLs required to execute the Easytrieve program on the mainframes. These JCLs are created in accordance with the settings done on the VScode plugin settings.

  The following options apply to the JCL generate functionality. To generate a JCL for the Easytrive code, specify the following:

  * **jobcard**

    A freeflow Jobcard field for the Easytrieve execution JCL. This could be any freeflow jobcard as specified y the programmer.

    ` //JOB01EZT JOB (123456789),'EZTPRGMR'` is the default setting.

    This jobcard will inserted in the beginning of the generated JCL

  * **cbaaload**

    The step libraries which are to be used in the JCL generation are to be specified here. Multiple libraries must be delimited by Semicolons.

    The libraries specified here would be used in the options table creation step as well as the Easytrieve compile and go step of the generated JCL.

  * **ezoptbl**

    Options table for the generated JCL for Easytrieve execution.
    A valid pre-created/compiled options table needs to be specified here. 

    * When `ezoptbl` is present, the `JCL generate` feature  generates only a compile and go step of the Easytrieve.
  
    * When `ezoptbl` is not present, the`JCL generate` feature also adds an options file creation step to the generated JCL.

    The options for this step are recovered from the Windows options file if `ezoptbl` is present. Otherwise, the Easytrieve default options are used.

  * **jobparm**

    Jobparm for the Easytrieve execution JCL (e.g. /*JOBPARM SYSAF=*)
    The default is set to empty

  * **eztdsn**

    DSN for the Easytrieve programs(*.EZT files) to be uploaded to.

    The default is set to empty.
  
    Specify if you want to upload Easytrieve code to separate mainframe dataset and refer the dataset to the generated JCL. In it's absence, the Easytrieve program would be placed in-line with the generated JCL.

  * **jcldsn**

    DSN for the generated JCL(generated *.JCL files) to be uploaded to

    The default is set to empty.

    Specify if you want to upload generated JCL to mainframe from VSCode

### Other settings

  * **Db_Name_Override**

    ODBC Datasource name or Database name for SSID PARM Override overrides the database name (e.g. SSID) specified in the source code useful when the local database (usded by the language server) name differs from the one used on the target system (used during execution).

    After changing this value, the language server requires restart of VS Code.

    The default of this setting is empty.

  * **env**

    This setting can be used to set any other needed environment variable. 
    
    It is necessary to edit this setting directly in the JSON as the type is a JSON object.

## Configuring
### Configuring the plug-in

Use the following procedure to set up the Options table file with the VS Code plugin. You can specify where the options are stored as an input for the compiler. 

_Perhaps we should add an example of how to specify where an option can be stored here._


 The language server feature uses the modified compiler and the Easytrieve Options Table. These options affect how the Easytrieve compiler works at runtime.

1. Create the file for the Options Table fields.

   **Example:**

   _add example here_

2. In Settings, select the Options Table field.
   
   [Options Table One](options1.png)

   [Options Table Two](options2.png)

   These options are subsequently picked up by the parcer component of the language server (compiler).

   **Notes:** 
   
   * Ensure that the options table has the `.def` extension
   
   * Ensure that the format is in the Unix end-of-lines by selecting the line feature
   
     _Add icon from the UI of this Line feature._  

   * Ensure that there is an empty line at the end of the file.

     These are requirements for the itop load which is used to compile the Options Table.

#### Options Table Setup

_Add description of Options Table Setup_
#### Macro Location Setup 

_Add description of Macro Location Setup_
#### Create a Zowe profile

To create profile in Zowe, we recommend you install the **Zowe Explorer plugin**. _(Add link to Zowe Explorer plugin)_

#### Interface Setup

_Link to Easytrieve Doc for Interface Setup_

### Configuring your Database

If the edited source code references a database, it is necessary to install and configure the proper database software the same way as for Easytrieve for Windows. It is also necessary to set up the `eztsql` option.

_Add links to database configuration from Easytrieve doc in Techdocs_

Database options to use with the plugin are the same as those options avaiable through the conventional Easytrieve for Windows interface. Database options include:

* DB2 
* ODBC

_Describe how to set up the database and add link to database setup in Techdocs documentation_


### Configuring JCL Generate Options

Follow these steps to configure JCL generate options:

1. Specify the following options:

   * `Cbaaload` 
     
      Step libraries for Options table creation and Easytrieve execution)
   * `Ezoptbl`

      An existing options table dataset residing on the mainframes
      
   * `Eztdsn` 
   
      An existing dataset to upload the created Easytrieve programs to
      
   * `Jcldsn`
   
      An existing dataset to upload the generated JCL to
      
   * `Jobcard`
   
      A freeflow jobcard which is to be used in the generated JCL
      
   * `Jobparm`
   
      Jobparm to be used in the generated JCL

2. Define the Input and Output files required for the execution of the Eadytrieve program. The IO files in the JCL format could either be specifed in the `<Easytrieve_program_name>.dsn` file within the workspace (or) in the user prompt which is displayed after the "generate JCL" contexual option which appeard on the right-click on the `*.ezt` file display.

   **Example:**
   
    For the Easytrieve program `employee.ezt`, the input file **salary** is to be specified as in the `employee.dsn` file.

   ```
   //SALARY    DD DSN=SITEID.DATE.SALARY,DISP=SHR
   ```
   Comments such as `//*THIS IS JUST A COMMENT` or other DD statements could also be added to the `<Easytrieve_program_name>.dsn` file.

   The common DD statements which are to be added to every generated JCL on the workspace are mentioned in the default.dsn file within the workspace.

   Comments such as `//*THIS IS JUST A COMMENT` or other DD statements could also be added to the `default.dsn` file.

### Upload EZT to mainframes

After generating the JCL, if the `Eztdsn` setting is specified with a valid dataset name, it is necessary to upload the Easytrieve program to the mainframe. 

In this case, the dataset name is referenced in the generated JCL and would be invalid if the program member does not reside in the specified dataset name. 

_What is invalid in this previous statement?_

If a dataset name is specified in the `Jcldsn` setting, the generated JCL is uploaded to the mainframe.

### Configuring plug-in for Windows (Optional)

- _add link_

## Using the Plugin

The plug-in is activated by opening a file with `.ezt` extension, or can be activated by selecting the Easytrieve language.

### Use Scenarios

_Add scenarios for using the plugin_
