# Easytrieve Language Support Plugin

 The Easytrieve Language Support plugin adds features and functionalites to Visual Studio Code which can help with editing and executing Easytrieve code both locally as well as when using Zowe on mainframe.   

 _Here we should make a statement about the benefits of using the plugin compared to just using Easytrieve directly._
 
 ## Table of Contents

* Summary
* Dependencies
* Basic Options
* Further Options
* Installing
* Configuring
  * Configuring the plugin
    * Option Table Setup
    * Macro Location Setup
    * Create a Zowe Profile
    * Interface Setup
  * Configuring your database
  * Configuring JCL Generate Options
  * Configuring the plugin for Windows
* Using
  * Use-case Scenarios
 
 ## Summary 
 * **LSP**
 
     The Easytrieve Language Support plugin provides the following subset of LSP (Language Server Protocol) functions:

     * **Source code validation**

       _Add detail to definition_

       Error and warning messages from a modified Easytrieve compiler are the same as when the source code is compiled under the same conditions (options table, database connection, etc.) 

     * **Hover**

       The hover functionality provides additional information to symbols (fields and filenames). Use the hover function by placing the cursor over the field name or file name. Information may be limited if the symbol is not defined in the edited file (i.e. symbol is defined in a macro).
     
     * **Go to definition**

       This feature makes it possible to jump to a specific location where a symbol (field or filename) is defined. This function is limited to a currently edited file. This feature is not available for symbols defined in a macro.

     * **Autocompletion** (in a specific context)

       Autocompletion can be used when a collon `:` is used after a filename. Enter **ctrl** + **space** to see a list of all fields defined for the file. _(Describe the limitations of this feature)_
 * **Options table editing**
    - LSP: only error checking available
 * **Local build and run task**

   _Add definition_
   
 * **Mainframe communication tools**

   _Add definition_

* **Easytrieve build-and-run**

   _Add definition_

* **Other basic plugin functionalities**

   Basic functionalities of the plugin include syntax coloring. _deleted part is decribed above_
## Dependencies
Currently, this plugin works only on Windows 10. For full functionality, other tools or plugins may be needed:

_Perhaps we should describe when/how these tools are useful for full functionality_

- Zowe CLI
- Zowe Explorer
- JCL Language support
- DB2 (client, connect, server)
- Oracle (client, server)
- Easytrieve for Windows

## Basic Settings

Basic options available with the plugin include the following categories:

* **Options Table**

  _add description of Options Table_

* **Macros**

  _add description of Macros_

* **eztsql**

  _add description of eztsql_

* **zoweProfile**

  _add description of zoweProfile_

**Options Table**

_Are the following notes to the user who is filling in the fields? If so, can we describe the procedure?_

  - Use the path to the file containing the options table definitions in text format. Use the `.def` extension.
  - Use the same format as is used as input for `etopload`. (**_link to EZT Techdocs_**)
  - Ensure to include an empty line at the end. 
  - The default is empty. This results in a W703 warning. - **_link to EZT Techdocs_**)
  - The relative path is evaluted against first root of the workspace
  - Ensure that you update whenever other values other than default values are applied in the options table for validating and parsing the code by the language server (plugin)

For more information, see [Create or Modify an Options Table](https://techdocs.broadcom.com/us/en/ca-mainframe-software/devops/ca-easytrieve-report-generator/11-6/using/workbench/configuration-manager/create-or-modify-an-options-table.html) in the Easytrieve documentation.

**Macros**


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
  

 

**eztsql** 

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

**zoweProfile**

  - Specify the zowe profile. To use the default profile, leave this field empty (default).  
    
    For more information, see [Zowe Explorer Profiles](https://docs.zowe.org/stable/user-guide/ze-profiles/).

  - Specify if you want VSCode to communicate with the mainframe (e.g. downloading macros, uploading generated JCLs)
  
    For more information, see [Creating a zosmf profile](https://docs.zowe.org/stable/getting-started/cli-getting-started/#creating-a-zosmf-profile).

## Further Options Available

**JCL generate options**
**_dicuss this with Saran_**

The following options apply to the JCL generate functionality. To generate a JCL for the Easytrive code, specify the following:

  * jobcard
  * cbaaload
  * ezoptbl 

- jobcard
  - VSCode description: Jobcard for the Easytrieve execution JCL
  - default "//JOB01EZT JOB (123456789),'EZTPRGMR'"
  - this jobcard will inserted in the beginning of the generated JCL
- cbaaload
  - VSCode description:  Libraries for steplib delimited by semicolon
  - default: "\*\*.CBAALOAD;\*\*.LOADLIB"
- ezoptbl
  - VSCode description: Options table for Easytrieve
  - default: "\*\*.EZOPTBL"
  - location of compiled options table on mainframe
- jobparm
  - VSCode description: Jobparm for the Easytrieve execution JCL (e.g. /*JOBPARM SYSAF=*)
  - default: empty
  - optional
- eztdsn
  - VSCode description:  DSN for the EZT programs to be uploaded to
  - default: empty
  - specify if you want to upload Easytrieve code to separate mainframe dataset instead of inlineing it into JCL
- jcldsn
  - VSCode description: DSN for the generated JCL to be uploaded to
  - default: empty
  - specify if you want to upload generated JCL to mainframe from VSCode

**Other options**

- Db_Name_Override
  - VSCode description: ODBC Datasource name or Database name for SSID PARM Override
  - default: empty
  - overrides the database name (e.g. SSID) specified in the source code useful when the local database (usded by the language server) name differs from the one used on the target system (used during execution)
  - after changing this value, the language server has to restarted, this means to restart VSCode (note: plan is to remove this requirement)

**_probably to be removed_**

- env: 
  - type: object
  - VSCode description:  addtional environment variables
  - this can be used to set any other needed environment variable
  - this has to be edited directly in JSON, because the type is JSON object
  - this option will be probably removed in the future, maybe even before release

## Installing 
The plugin can be installed from the VSCode market. For more information about installation from the VS Code marketplace, see [Extension Marketplace](#https://code.visualstudio.com/docs/editor/extension-marketplace). Right after installation the basic functionality is available. For full functionlity  other tools or plugins may be needed to install and cofigure.


## Configuring

### Configuring the plugin
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

- job card
- job parm

### Configuring plug-in for Windows (Optional)

- add link 

## Using the Plugin

The plug-in is activated by opening a file with `.ezt` extension, or can be activated by selecting the Easytrieve language.

### Use Scenarios

_Add scenarios for using the plugin_

