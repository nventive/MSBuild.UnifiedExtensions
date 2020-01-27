# nventive.Nimue.TargetFrameworks.Filter
A set of MSBuild targets allowing to skip the build of target frameworks not found in the build environment.
This can be useful when building cross-targetted libraries and samples (for example: a library targetting UAP and Xamarin.iOS with a sample app)

## How to use it?
1. Install the NuGet package in the relevant projects
1. As a build parameter, set the `SkipUnknownFrameworks` variable to true
1. Build

*Note*: this feature is purposefully disabled when building inside of Visual Studio.