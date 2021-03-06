# XML Documentation Comments Support for AL language in Visual Studio Code

Generate XML documentation comments for AL language in Visual Studio Code and generate markdown files from Source Code.

## Usage
### Generate context aware XML documentation comments
Type `///` in AL source code, it auto-generates an context aware XML documentation comment like this:

![Generate context aware XML documentation comments][GenerateXmlDoc]

In addition to the regular documentation activity you can:
 - Add new lines in existing XML Documentation comment section. (`///` will automatically added.)
 - Use [Snippets](#Snippets) directly inside the XML Documentation comment section.

### Generate markdown files from XML documentation comments
There are two commands available to generate markdown files from XML documentation:

| Command | Description | 
| --- | --- |
| `AL DOC: Generate markdown documentation` | Create markdown documentation file for the currently opened AL source code file. |
| `AL DOC: Generate markdown documentation for directory` | Create markdown documentation files for all AL source code files in the currently opened directory. |

> **Note**<br>All commands start with `AL DOC` prefix to make it easier to find them.

![Generate markdown files from XML documentation comments][GenerateMDDoc]

Generate markdown documentation files, based on the XML documentation in AL source code. For each object file (e.g. `MyCodeunit.Codeunit.al`) a subdirectory inside the export directory will be created.
Each procedure and trigger method is creating a single file (e.g. `DoSomething.al`) inside the subdirectory. Additionally an index file (`index.md`) will be created per object file and contains a list of every documented element in the source file.

> **Note**<br>This feature exports all valid XML documentation from objects with access modifier `Public` (or not set) and Subtype `Normal` (or not set).<br><br>Additionally warnings will be shown in the output channel in Visual Studio Code to show missing documentations.

### Show summary tag description from XML documentation comments as tooltip
If `enableSummaryHover` configuration is activated (default) every time hovering over a procedure in your AL source code the XML documentation in the source file or symbols will be searched and presented as tooltip.

![Show XML documentation summary as Tooltip][SummaryHover]

> **Note**<br>Currently only procedures are supported and only the `<summary>` tag will be presented as tooltip.

> **Important**<br>Due to possible accessibility limitations of symbol files (`showmycode` in AL project `app.json`, etc.) it's not possible to retrieve the XML documentation comments for dependencies in this case.

### Diagnostic & Quick Fix actions
If `checkProcedureDocumentation` configuration is activated (default) every AL source file in current workspace will be checked for complete procedure documentation.
Incomplete or missing procedure documentations are added as diagnostic entries (information level) and providing quick fix actions to solve.

![Diagnostic & Quick Fix actions][DiagnosticsQuickFix]

#### Diagnostic Codes
Currently the following diagnostic codes and associated actions are implemented:

| Diagnostic Code | Description | Quick Fix Action |
| --- | --- | --- |
| DOC0001 | XML documentation for procedure is missing. | Add documentation |
| DOC0002 | `<summary>` documentation for procedure is missing. | Add `<summary>` documentation |
| DOC0010 | `<param>` documentation for procedure is missing. | Add `<param>` documentation |
| DOC0011 | `<param>` documentation exist but referred parameter does not exist. | _no quick fix available_ |
| DOC0020 | `<returns>` documentation for procedure is missing. | Add `<returns>` documentation |

### Snippets
Three snippets are included into the extension:
#### Summary XML documentation
`docsummary` snippet adds simple `<summary>` xml documentation comment:
```c#
    /// <summary>
    /// This is the description of a specific procedure, trigger or object.
    /// </summary>
```
#### Example code XML documentation
`docexamplecode` snippet adds `<example>` xml documentation comment:
```c#
    /// <example>
    /// This is the description of an example
    /// <code>
    /// if (i <> y) then
    ///   DoSomething(i, y);
    /// </code>
    /// </example>
```
#### Remarks XML documentation
`docremarks` snippet adds `<remarks>` xml documentation comment:
```c#
    /// <remarks>
    /// This are some specific remarks for the documented procedure.
    /// </remarks>
```

## Installation
1. Install Visual Studio Code 1.44.0 or higher
2. Launch Visual Studio Code
3. From the extension view Ctrl-Shift-X (Windows, Linux) or Cmd-Shift-X (macOS)
4. Search and Choose the extension AL XML Documentation
5. Reload Visual Studio Code

## Setup
The menu under File > Preferences (Code > Preferences on Mac) provides entries to configure user and workspace settings. 

The following configuration parameters are available:

| Configuration Parameter | Description | Default Value |
| --- | --- | --- |
| `procedureTypes` | Sets the list of procedure types (e.g. event publisher, tests) checked. | `not defined` (all procedures activated) |
| `enableDocComments` | Specifies whether typing `///` will insert the xml documentation structure. | `true` |
| `markdown_path` | Specifies the path where the markdown files should be created. | `doc` folder in workspace root directory |
| `verbose` | Specifies whether detailed information should be output during markdown creation. | `false` | 
| `exportScope` | Specifies whether only global procedures (config value: `global`) or whether all procedures (config value: `all`) should be exported as markdown. | `global` |
| `enableSummaryHover` | Specifies whether `<summary>` description should be shown on procedures as tooltip. | `true` |
| `askEnableCheckProcedureDocumentation` | Specifies whether a confirmation will appear to enable procedure documentation for each workspace. | `false` | 
| `checkProcedureDocumentation` | Specifies whether xml documentation should be checked inside current workspace. | `true` | 

> **Important**<br>The object directory (e.g. `doc\mycodeunit.codeunit.al\`) will be deleted if already exist.

### settings.json
```json 
{
    "bdev-al-xml-doc.procedureTypes": [
        "Global Procedures",
        "Local Procedures",
        "Internal Procedures"
    ],   
    "bdev-al-xml-doc.markdown_path": "C:/Documentation/",
    "bdev-al-xml-doc.verbose": true,
    "bdev-al-xml-doc.exportScope": "all",
    "bdev-al-xml-doc.enableSummaryHover": true,
    "bdev-al-xml-doc.askEnableCheckProcedureDocumentation": true    
}
```

> **Note**<br>`markdown_path` does support `${workspaceFolder}` as an placeholder.<br><br>_Example_<br>`"bdev-al-xml-doc.markdown_path": "${workspaceFolder}/documentations/src/"`<br><br>

## Supported Languages
This extension is only processing AL language source code files.

## Supported AL Keywords
| Object Type | Supported |
| --- | :---: |
| `procedure` | ![Supported] |
| `local procedure` | ![Supported] |
| `internal procedure` | ![Supported] |
| `trigger` | ![Supported] |

> **Note**<br>The purpose of the AL XML Documentation is to document your AL Source Code, not to document structures, controls or table fields.<br><br>Therefor it's currently not planned to add support for AL keywords, other the currently supported.

## Supported AL Objects
| Object Type | Supported |
| --- | :---: |
| `codeunit` | ![Supported] |
| `table` | ![Supported] |
| `page` | ![Supported] |
| `enum` | ![Supported] |
| `xmlport` | ![Supported] |
| `interface` | ![Supported] |
| `table extension` | ![Supported] |
| `page extension` | ![Supported] |
| `enum extension` | ![Supported] |
| `page customization` | ![NotSupport] |
| `profile` | ![NotSupport] |

## Supported Documentation XML Tags

| XML Tag | Supported |
| --- | :---: |
| `summary` | ![Supported] |
| `param` | ![Supported] |
| `returns` | ![Supported] |
| `remarks` | ![Supported] |
| `example` | ![Supported] |

## System Requirements
 - Visual Studio Code 1.44.0 (or higher) - [Download here](https://code.visualstudio.com/Download)
 - .NET Core 3.0 (or higher) - [Download here](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## License
This extension is licensed under the [MIT License](https://github.com/365businessdev/vscode-alxmldocumentation/blob/dev/LICENSE.txt).

[GenerateXmlDoc]: https://github.com/365businessdev/vscode-alxmldocumentation/blob/master/doc/AddXmlDocComment.gif?raw=true "Generate context aware XML documentation comments"
[GenerateMDDoc]: https://github.com/365businessdev/vscode-alxmldocumentation/blob/master/doc/GenerateMarkdownDoc.gif?raw=true  "Generate markdown files from XML documentation comments"
[SummaryHover]: https://github.com/365businessdev/vscode-alxmldocumentation/blob/master/doc/HoverProcedureDescription.gif?raw=true  "Generate markdown files from XML documentation comments"
[DiagnosticsQuickFix]: https://github.com/365businessdev/vscode-alxmldocumentation/blob/master/doc/ALCheckDocumentationDiagnosticsQuickFix.gif?raw=true  "Diagnostics and Quick Fix"
[Supported]: https://cdn4.iconfinder.com/data/icons/icocentre-free-icons/137/f-check_256-16.png "Supported"
[NotSupport]: https://cdn2.iconfinder.com/data/icons/circular%20icons/no.png "Not Supported"