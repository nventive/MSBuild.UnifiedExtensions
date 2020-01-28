# nventive.Nimue.Versioning
A set of MSBuild targets allowing to easily set the application version for different platforms (UAP, Xamarin.Android, Xamarin.iOS and WASM). 
At build time, these targets look for and update the corresponding manifest file and update it with the given version.
The supported manifest files are:
- Package.appxmanifest (UAP)
- Info.plist (Xamarin.iOS)
- AndroidManifest.xml (Xamarin.Android)
- Version.txt (WASM)

## How to use it?
1. Install the NuGet package in the relevant projects
1. As a build parameter, set the `ApplicationVersion` variable to the desired version (format should be `Major.Minor.Patch`)

*Note*: this feature is purposefully disabled when building inside of Visual Studio.

###Variables
The available variables are:
- `ApplicationVersion`: used to set the version; required unless GitVersion is used
- `ApplicationBuildNumber`: used to set the build number (UAP/WASM), version code (Xamarin.Android) or bundle version (Xamarin.iOS); optional

###GitVersion
If the project is being built after a GitVersion command, the variable `GitVersion_MajorMinorPatch` is automatically used for the `ApplicationVersion` variable, unless explicitely set

###Build number
If the project is inside a Git repository and the Git executable exists in the build environment, the total commit count will be used for the `ApplicationBuildNumber` variable, unless explicitely set