# nventive.Nimue.Versioning
A set of MSBuild targets allowing to easily set the application version for different platforms (UAP, Xamarin.Android, Xamarin.iOS and WASM). 

## Why should I use it?
When building a UAP, Xamarin.iOS or Xamarin.Android application, there's no easy, built-in and unified way to set the version of the application dynamically.
Using this package, you will be able to set the different versions for the different application manifests using 2 variables. Here's a rundown of what's supported:
- For UAP, the version in the Package.appxmanifest is set with a version matching [Microsoft's version format](https://support.microsoft.com/en-ca/help/556041)
- For Xamarin.iOS, the `CFBundleShortVersionString` is updated with the version given and `CFBundleVersion` with the build number in the Info.plist
- For Xamarin.Android, the `versionName` is updated with the version given and the `versionCode` with the build number in AndroidManifest.xml
- For WASM applications, the Version.txt file is updated with the same version used for UAP

## How to use it?
Simply install the NuGet package in the your UAP, Xamarin.Android and Xamarin.iOS application projects. Once there, there's several options:

### Manually

#### Project level
In the csproj file, add the following:
```xml
<PropertyGroup>
    <ApplicationVersion>1.2.3</ApplicationVersion>
    <ApplicationBuildNumber>123</ApplicationBuildNumber>
</PropertyGroup>
```
This will, at build time, update the manifest with the proper values.

#### Command line
When building the solution with the MSBuild command line, use the following:
```cmd
msbuild.exe [...] /p:ApplicationVersion=1.2.3 /p:ApplicationBuildNumber=123
```

#### Azure Pipelines YAML
When building the solution with Azure Pipelines, if GitVersion is used and run before the build step, `ApplicationVersion` is automatically set to `$(GITVERSION_MAJORMINORPATCH)`. The variables can also be set like so
```yml
variables:
- name: ApplicationVersion
  value: 1.2.3
- name: ApplicationBuildNumber
  value: 123
```

### Automatically

When used outside of Visual Studio (in a CI environment for instance), the versions can be managed automatically under certain circumstances:
- `ApplicationBuildNumber` is calculated automatically using the number of commits when the code is inside of a Git repository and Git is found on the build machine
- When running the build in Azure Pipelines after running [GitVersion](https://marketplace.visualstudio.com/items?itemName=gittools.gitversion), the calculated [`MajorMinorPatch`](https://gitversion.net/docs/more-info/variables) version will be used as the `ApplicationVersion`

*Remark: Those mechanisms are only used if the relevant variable is not manually set*
