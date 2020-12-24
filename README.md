# BrandLink.Standards

This repository contains the configuration and guidelines for automated linting of C# projects. It is designed to be installed as a [git submodule](https://blog.github.com/2016-02-01-working-with-submodules/) into your solution.

## Installation.

This installation guide assumes that your solution conforms to the following structure: 

``` bash
solution.sln
readme.md
.gitignore
+---> src
+      +
+      +---> project
+      +   +
+      +   +---> project.csproj
+      +
+      +---> project
+          +
+          +---> project.csproj
+
+---> tests
       +
       +---> project.tests
       +   +
       +   +---> project.tests.csproj
       +
       +---> project.tests
           +
           +---> project.tests.csproj
```

If the solution does not conform to this structure you will have to update it to do so.

### Adding the Submodule

To add BrandLink.Standards as a submodule of your project. In the project repository type:

``` bash
git submodule add https://github.com/BrandLink/BrandLink.Standards standards
```

At this point, you’ll have a **standards** folder inside your project, but if you were to peek inside that folder, depending on your version of Git, you might see… nothing.

Newer versions of Git will do this automatically, but older versions will require you to explicitly tell Git to download the contents of **standards**:

``` bash
git submodule update --init --recursive
```

If everything looks good, you can commit this change and you’ll have a **standards** folder in your project repository with all the content from the BrandLink.Standards repository.

### Updating the Submodule. 

Since the submodule is stored in a separate repository you may find at times updates have been made to the linting rules that require you to update your copy. The command below will allow you to do so:

``` bash
git submodule update --init --recursive
git submodule foreach git pull origin master
```

### Wiring up the Linting Tools

There are three tools contained within the submodule that will help to automatically promote and enforce coding standards and consistancy:
- [.gitattributes](https://git-scm.com/docs/gitattributes)
- [.editorconfig](https://docs.microsoft.com/en-us/visualstudio/ide/create-portable-custom-editor-options?view=vs-2017)
- [StyleCop Analyzers](https://github.com/DotNetAnalyzers/StyleCopAnalyzers)

### Gitattributes
`.gitattributes` files are used to do things like specify separate merge strategies for individual files or directories in your project, tell Git how to diff non-text files, or have Git filter content before you check it into or out of Git. The default attributes are configured to safely allow cross platform development.

>**Note: `.gitattributes` relies on a [physical file path hierarchy](https://git-scm.com/docs/gitattributes) to work so after installing or updating the submodule you need to copy the `.gitattributes` file to the solution root before adding to Visual Studio.**

#### EditorConfig 

Settings in `.editorconfig` files enable you to maintain consistent coding styles and settings in a codebase, such as indent style, tab width, end of line characters, encoding, and more, regardless of the editor or IDE you use. For example, when coding in C#, if your codebase has a convention to prefer that indents always consist of five space characters, documents use UTF-8 encoding, and each line always ends with a CR/LF, you can configure an `.editorconfig` file to do that.

Adding an `.editorconfig` file to your project or codebase does not convert existing styles to the new ones. For example, if you have indents in your file that are formatted with tabs, and you add an  `.editorconfig` file that indents with spaces, the indent characters are not automatically converted to spaces. However, any new lines of code are formatted according to the `.editorconfig` file. Additionally, if you format the document using  <kbd>Ctrl+K, Ctrl+E</kbd>), the settings in the `.editorconfig` file are applied to existing lines of code.

>**Note: `.editorconfig` relies on a [physical file path hierarchy](https://editorconfig.org/#file-location) to work so after installing or updating the submodule you need to copy the `.editorconfig` file to the solution root before adding to Visual Studio.**

To add an `.editorconfig` file to your solution open the solution in Visual Studio. Select the solution node and right-click.

From the menu bar, choose **Project > Add Existing Item**, or press <kbd>Shift+Alt+A</kbd>.

The Add Existing Item dialog box opens. Use this to navigate to your new **standards** folder and select the `.editorconfig` file.

An `.editorconfig` file appears in Solution Explorer, and it opens in the editor.

![EditorConfig file in solution explorer.](images/editorconfig-in-solution-explorer.png)

Add the following to your src .csproj file to ensure that the `.editorconfig` and `.gitattributes` files are automatically copied into the solution root.

```xml
  <ItemGroup>
    <!--Shared config files that have to exist at root level.-->
    <ConfigFilesToCopy Include="..\..\standards\.editorconfig;..\..\standards\.gitattributes" />
  </ItemGroup>

  <!--Ensures our config files are up to date.-->
  <Target Name="CopyFiles" BeforeTargets="Build">
    <Copy SourceFiles="@(ConfigFilesToCopy)" DestinationFolder="..\..\" />
  </Target>
```

#### StyleCop Analyzers

StyleCop Analyzers are Roslyn Analyzer that contain an implementation of the StyleCop rules using the .NET Compiler Platform. Where possible, code fixes are also provided to simplify the process of correcting violations.

StyleCopAnalyzers can be installed using the NuGet command line or the NuGet Package Manager in Visual Studio 2017.

Install using the command line:

``` bash
Install-Package StyleCop.Analyzers
```

Installing via the package manager:

![stylecop analyzers-via-nuget](images/stylecop-analyzers-via-nuget.png)

Once the Nuget package is installed you will should add the following configuration files to the **Solution Items** folder you created when installing the `.editorconfig` file so you can easily view the contents. 

- `brandlink.ruleset`
- `brandlink.tests.ruleset`
- `stylecop.json`

These files tell StyleCop what rules to enforce and will have to be manually added to each project. **right-click > Edit [YOUR_PROJECT_NAME].csproj**

``` xml
<!--Use the 'brandlink.tests.ruleset' for your test projects-->
<PropertyGroup>
  <CodeAnalysisRuleSet>..\..\standards\brandlink.ruleset</CodeAnalysisRuleSet>
</PropertyGroup>

<ItemGroup>
  <AdditionalFiles Include="..\..\standards\stylecop.json" />
</ItemGroup>
```

An up-to-date list of which StyleCop rules are implemented and which have code fixes can be found [here](https://dotnetanalyzers.github.io/StyleCopAnalyzers/).
