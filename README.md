# SvSoft.MSBuild.CheckUnnecessaryUsings

Enable the compiler check for [unnecessary usings (IDE0005)](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules/ide0005) in CLI builds just by including this package.

```xml
<PackageReference Include="SvSoft.MSBuild.CheckUnnecessaryUsings" Version="1.0.0" PrivateAssets="all" />
```

The package will enforce style checking in the build by setting `EnforceCodeStyleInBuild`.
(You may thus get additional non-IDE0005 style-related warnings, depending on your style settings.)
It will also set the diagnostic severity of IDE0005 to `warning` for the consuming project via a global analyzer config file.
The severity will be overridden by default by any `.globalconfig` file or any other global analyzer config file with a `global_level` greater than 10.

# Background

There is a [long standing issue in Roslyn](https://github.com/dotnet/roslyn/issues/41640) that prevents the CLI build from checking for unnecessary usings unless XML doc generation is turned on.
Yes, I know, crazy.
There is a [comparatively simple workaround](https://github.com/dotnet/roslyn/issues/41640#issuecomment-985780130).
But it has some disadvantages:

* causes an unwanted and unnecessary potentially empty XML doc file to be included in the output folder
* has to be manually disabled as soon as the project _does_ want real XML doc generation

In contrast, this solution does not leave any unnecessary files in the output folder.
It will also only trigger when `GenerateDocumentationFile` is `false` (default) in the consuming project. As soon as XML doc file generation is enabled by setting it to true, the workaround will no longer trigger and thus not have any unwanted side-effects.