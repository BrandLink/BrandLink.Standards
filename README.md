# BrandLink.Standards

This repository contains the configuration and guidelines for automated linting of C# projects. It is designed to be installed as a [git submodule](https://blog.github.com/2016-02-01-working-with-submodules/).

## Installation.

This installation guide assumes that your solution conforms to the following structure: 

``` txt
solution.sln
readme.md
.gitattributes
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

### Adding the Submodule

To add BrandLink.Standards as a submodule of your project. In the project repository type:

``` bash
git submodule add https://github.com/BrandLink/BrandLink.Standards standards
```

At this point, you’ll have a standards folder inside your project, but if you were to peek inside that folder, depending on your version of Git, you might see… nothing.

Newer versions of Git will do this automatically, but older versions will require you to explicitly tell Git to download the contents of rock:

``` bash
git submodule update --init --recursive
```

If everything looks good, you can commit this change and you’ll have a standards folder in your project repository with all the content from the BrandLink.Standards repository.

### Updating the Submodule. 

Since the submodule is stored in a separate repository you may find at times updates have been made to the linting rules that require you to update your copy. The same command as above will allow you to do so:

``` bash
git submodule update --init --recursive
```

### Wiring up the Linting Tools

TODO: Write me.
