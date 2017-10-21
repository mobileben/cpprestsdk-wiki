# How to setup, build, and run tests on Windows

## Setup [Git](http://git-scm.com/)

If you use PowerShell, we recommend installing Posh-Git ([here](Posh-git) is how).  

If you use cmd.exe, open the **Developer Command Prompt for VS2013** ([here](Setting-up-Visual-Studio-command-line-prompt) is how)  

## Get the source

```bash
git clone https://github.com/Microsoft/cpprestsdk.git
```

## Build from source

There are two ways you can build on Windows, from the command line or from Visual Studio. Please note the C++ Rest SDK depends on Boost and OpenSSL.

### Visual Studio solution files

The easiest way to build is by using the Visual Studio solution files. They contain the product code and samples. Please note none of the tests are included in the solution files; if you wish to build the tests you will need to follow the command line instructions or create your own solution files.  

The Visual Studio solution files are located in the top of the repository. Different files exist for Visual Studio 2013 and 2015\. For example the solution file called **cpprestsdk120.sln** contains the projects development with Visual Studio 2013\. To build simply open the solution file you need and away you go! The source code is structured into different shared projects based on platform. For example the 'common' project contains all the public headers and the source code common to all platforms. The 'win32' project contains source specific to Windows Desktop, 'winrt' specific to the Windows Runtime, etc...  

Note: Visual Studio 2013 Express will be unable to load the project files, because that particular SKU does not support Shared Items projects. You can either install Visual Studio 2015 Community and build the 2013 binaries through the 2015 IDE or use the command line instructions below.

Note: We use the Boost and OpenSSL libraries on Windows; when you build the solution file the first time, it will automatically download all the needed libraries. This can take a while, depending on your connection speed. Furthermore, it may appear that Visual Studio hangs while downloading. This is expected and the download should not be affected.  

### Command line

The more complex and complete way to build can be performed from the command line. However, you will need to provide all of the dependencies that we require. The easiest way to provide these libraries is to build once with Visual Studio following the above instructions (this will download appropriate NuGet packages). However, you can also provide them via the environment variables LIB, LIBPATH, and INCLUDE. If you are using Powershell you can also use the [NuGet command line](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe) to download all the package dependencies.

```cmd
.\NuGet.exe restore .\cpprestsdk120.sln
```

After obtaining all the dependencies, navigate to the Release directory in the repository while in a developer prompt and type:  

```cmd
msbuild dirs.proj /p:Configuration=Debug /p:Platform=x64 /p:UseEnv=true
```

(You might need to change the Configuration and Platform parameters as needed)  

### Targeting XP

Please note if you are using the C++ Rest SDK built from source yourself and targeting Windows XP you need to include the define CPPREST_TARGET_XP. If you are using our NuGet package for release 2.4.0 and later this is automatically done for you.  

## Run the tests (Command Line Only)

1.  Navigate to the Casablanca **Release\tests\** directory
2.  Build following the command line instructions above, i.e. **msbuild dirs.proj /p:Configuration=Debug /p:Platform=x64 /p:UseEnv=true**
3.  After building completed, you can run the tests.
4.  Navigate to the Binaries directory and select the correct folder based on the configuration you built for
5.  Type the following command: **TestRunner.exe**
6.  You can now see all of the options available to run tests
7.  For example if we want to run all of the tests we can enter the following command:

```cmd
TestRunner.exe *test* 
```

Or if you wanted to only run the json tests use the following:  

```cmd
TestRunner.exe JSON120_test.dll
```