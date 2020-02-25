# nventive.Nimue.Building.Light
A set of MSBuild targets allowing to lighten up the build process.

## Why should I use it?
When building a UAP, Xamarin.iOS or Xamarin.Android application for a Pull Request, the build time can be quite long, especially when using things like LLVM or the .Net native compiler.
The goal of this library is to allow for faster PR builds by disabling time-consuming options:
- iOS: Disables LLVM, symbol strip, PNG optimization and symbols creation
- Android: Disables LLVM and AOT
- UAP: Prevent .Net native toolchain from being used
- All: Disables the C# [`optimize`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/compiler-options/optimize-compiler-option) option

Given the changes made, the binaries generated should not be used in production as they will be missing major optimizations.

## How to use it?
Simply install the NuGet package in the your UAP, Xamarin.Android and Xamarin.iOS application projects. Once this is done, the feature can be enabled with the `IsLightBuild` flag.

### Recommended: Azure Pipelines YAML
In your build definition, add a new variable like so:
```yml
variables:
- name: IsLightBuild
  value: $[eq(variables['Build.Reason'], 'PullRequest')]
```

This will ensures all the PR builds are run with this feature enabled.

### Project level
In the csproj file, add the following:
```xml
<PropertyGroup>
    <IsLightBuild>true</IsLightBuild>
</PropertyGroup>
```
*Note: this method should probably not be used*

### Command line
When building the solution with the MSBuild command line, use the following:
```cmd
msbuild.exe [...] /p:IsLightBuild=true
```