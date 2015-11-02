# How to setup, build, and run tests on Windows

## Setup [Git](http://git-scm.com/)

If you use PowerShell, we recommend installing Posh-Git ([here](https://casablanca.codeplex.com/wikipage?title=Posh-Git&referringTitle=How%20to%20setup%2c%20build%2c%20and%20run%20tests%20on%20Windows) is how).  

If you use cmd.exe, open the **Developer Command Prompt for VS2013** ([here](https://casablanca.codeplex.com/wikipage?title=Developer%20Command%20Prompt%20for%20VS&referringTitle=How%20to%20setup%2c%20build%2c%20and%20run%20tests%20on%20Windows) is how)  

## Get the source

```bash
git clone https://git01.codeplex.com/casablanca
```

This will clone Casablanca source into a directory named _casablanca_.  
Going forward, you will want to pull from the _master_ branch, which will always contain the last known release. The _development_ branch contains the latest work but is not stable.  

If you're using PowerShell, you can now set up VC++ compiler toolset by running the **setup_ps_env_VS2013.ps1** script.  

## Build from source

There are two ways you can build on Windows, from the command line or from Visual Studio. Please note the C++ Rest SDK depends on Boost and OpenSSL. The first time building in Visual Studio will take some time to download the NuGet package dependencies.  

### Visual Studio solution files

The easiest way to build is by using the Visual Studio solution files. They contain the product code and samples. Please note none of the tests are included in the solution files; if you wish to build the tests you will need to follow the command line instructions or create your own solution files.  

The Visual Studio solution files are located in the top of the repository. Different files exist for Visual Studio 2013 and 2015\. For example the solution file called **cpprestsdk120.sln** contains the projects development with Visual Studio 2013\. To build simply open the solution file you need and away you go! The source code is structured into different shared projects based on platform. For example the 'common' project contains all the public headers and the source code common to all platforms. The 'win32' project contains source specific to Windows Desktop, 'winrt' specific to the Windows Runtime, etc...  

Note: We use the Boost and OpenSSL libraries on Windows; when you build the solution file the first time, it will automatically download all the needed libraries. This can take a while, depending on your connection speed. Furthermore, it may appear that Visual Studio hangs while downloading. This is expected and the download should not be affected.  

### Command line

The more complex and complete way to build can be performed from the command line. However, you will need to provide all of the dependencies that we require. The easiest way to provide these libraries is to build once with Visual Studio following the above instructions (this will download appropriate NuGet packages). However, you can also provide them via the environment variables LIB, LIBPATH, and INCLUDE. If you are using Powershell you can also use the [NuGet command line](http://docs.nuget.org/docs/reference/command-line-reference) to download all the package dependencies.  

```cmd
.\NuGet.exe restore .\cpprestsdk120.sln
```

After obtaining all the dependencies, navigate to the Release directory in the repository and type:  

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