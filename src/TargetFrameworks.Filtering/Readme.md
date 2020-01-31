# nventive.Nimue.TargetFrameworks.Filter
A set of MSBuild targets allowing to skip the build of target frameworks not found in the build environment.

## Why would I use it?
This package can be useful when building cross-targetted libraries and samples. Here's an example of utilisation:

We have a solution A with :
- Library B, targetting UAP and Xamarin.iOS
- Sample application S1 for UAP, with a project reference to B
- Sample application S2 for Xamarin.iOS, with a project reference to B

When building either of the sample applications, B will be built for all the target frameworks. Currently, a Xamarin.iOS application can only be built on macOS and the UAP target framework can only be found on Windows. Because of this, when building S2 on macOS, the build of B will fail due to UAP not being resolved. In this scenario, one of the solution would be to fiddle with B to make sure that only the Xamarin.iOS version is built when building on macOS.

That's where this package comes in: when installing it and configuring it, the target frameworks that cannot be resolved on the build machine will simply not be built. 

*Note: If the feature filters out all target frameworks, an error will occur.*

**Important: Enabling this feature will produce invalid NuGet packages missing skipped frameworks binaries. Those packages should not be used.**

## How to use it?
Install the NuGet package in the multi-targetted libraries containing problematic target frameworks; the most common ones are UAP and Xamarin.iOS but Xamarin.Android can also be a candidate, since its installation is optional. Once this is done, the feature can be enabled with the `SkipUnknowFrameworks` flag

### Project level
In the csproj file, add the following:
```xml
<PropertyGroup>
    <SkipUnknowFrameworks>true</SkipUnknowFrameworks>
</PropertyGroup>
```
*Note: this method should probably not be used*

### Command line
When building the solution with the MSBuild command line, use the following:
```cmd
msbuild.exe [...] /p:SkipUnknowFrameworks=true
```

### Azure Pipelines YAML
Add a new variable like so:
```yml
variables:
- name: SkipUnknowFrameworks
  value: true
```
*Note: this variable shouldn't be set at the top-level, as it could affect the creation of the NuGet packages*