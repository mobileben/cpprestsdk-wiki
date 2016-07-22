# What is NuGet?

From the [NuGet website:](http://docs.nuget.org/docs/start-here/overview)

*   NuGet is a Visual Studio extension that makes it easy to add, remove, and update libraries and tools in Visual Studio projects.
*   When you install a package, NuGet copies files to your solution and automatically makes whatever changes are needed, such as adding references, includes, setting up libs, and dlls. This is all done automatically even if you adjust your target toolset making it much easier than manually having to manually deal with yourself. If you decide to remove the library, NuGet removes files and reverses whatever changes it made in your project so that no clutter is left.

# How to add the C++ REST SDK to a project using NuGet

*   Open Visual Studio and create a new project or open an existing project.
*   Go to **Tools > Extensions and Updates**, and make sure that the NuGet Package Manager installed and it is version 2.5 or later. (If not, you can install and update the NuGet manager from this window) ![NuGetPackageManagerInExtensionsAndUpdates.png](resources/NuGetPackageManagerInExtensionsAndUpdates.png)
*   Right click on the project and select **Manage NuGet Packages**. From this window you can add, remove and update packages.
*   Search for "cpprestsdk" (the C++ REST SDK/Casablanca) and click install

![SearchingCasablancaInNuGet.png](resources/SearchingCasablancaInNuGet.png)

*   That's it! The SDK is downloaded, installed, and the project properties including header files and library paths have been set for you. You can install NuGet packages on Visual Studio 2013 and 2015.

*   To get started with your first application using the C++ REST SDK, we recommend you start with the [http_client Tutorial](Getting-Started-Tutorial).

**Note: Currently the NuGet packages for C++ are cached on a per-solution basis. If you would like support for global package installation, please voice your feedback on the [NuGet discussion forum](https://nuget.codeplex.com/discussions/429991)**