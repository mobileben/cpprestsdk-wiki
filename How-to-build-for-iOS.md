# How to build and setup on iOS

First, follow all the instructions on [Setup and Build on OSX](How-to-build-for-Mac-OS-X). While some steps could be skipped if you _only_ want to build for iOS, it is useful to diagnose problems during the initial iOS project setup by replicating it on OSX.  

The C++ REST SDK depends on Boost and OpenSSL when used on iOS. It is a non-trivial task to cross-compile libraries for iOS, however there are scripts available online with nonrestrictive licenses to compile many popular libraries -- among these libraries are Boost and OpenSSL.

If you have pre-builts for the Boost or OpenSSL, you can place these in the `Build_iOS` directory. Boost will work with either a framework or static versions of the lib.

If `boost.framework`/`boost` or `openssl` directories do not exist, then `configure.sh` will download and build the library.

If using static lib version of boost, the directory structure must be:

> boost/include
> boost/lib  

This document will walk through the steps to build the C++ REST SDK and its dependencies into a form suitable for use with iOS applications.  

For this walkthrough, we assume you are working within the `Build_iOS` directory of the project.  

```
git clone https://github.com/Microsoft/cpprestsdk.git casablanca
pushd casablanca/Build_iOS
./configure.sh
```

This will produce a universal static library called "libcpprest.a" for the x86_64 simulator, arm64, and arm64e (64-bit architectures).

`configure.sh` accepts arguments to customize the generated static library. Acceptable arguments are listed below.

```
Usage: configure.sh [-build_type type] [-deployment_target version] [-config_only] [-include_32bit] [-no_bitcode]
       -build_type defines the CMAKE_BUILD_TYPE used. Defaults to Release.
       -deployment_target defines minimum iOS Deployment Target. The default is dependent on ios.toolchain.cmake and currently defaults to 8.0
       -config_only only configures cmake (no make invoked).
       -include_32bit includes the 32-bit arm architectures.
       -no_bitcode disables bitcode
```

The following examples show possible configurations:

Debug build type
```
./configure.sh -build_type Debug
```

32-bit and 64-bit architectures with iOS 10.0 as the minimum deployment
```
./configure.sh -deployment_target 10.0 -include_32bit
```

Disabled bitcode without making after configure
```
./configure.sh -no_bitcode -config_only
```

## Built library

The built static lib will be placed in the directory

`Build_iOS/build.TYPE.ios/lib`

The include files will be copied to

`Build_iOS/build.TYPE.ios/include`

Where `TYPE` is the `CMAKE_BUILD_TYPE` used to configure cmake. This is set via the `-build_type` argument to `configure.sh`

## Using the C++ REST SDK

You will need to link against the following from your project:  

*   build.debug/libcpprest.a
*   boost.framework (if using boost framework)
*	boost directory containing static versions of the boost libraries
*   openssl/lib/libcrypto.a
*   openssl/lib/libssl.a
*   libiconv.dylib (Available within the default list of libraries to link)
*   Security.framework (Available within the default list of libraries to link)

You will also need to add the following paths as additional include directories:  

*   ../Debug/include
*   boost.framework/Headers (if boost.framework used)
*	boost/include (if boost static libs )
*   openssl/include

This should allow you to reference and use the C++ REST SDK from your C++ and Objective-C++ source files. Note: you should change all .m files in your project to .mm files, because even if the source file itself does not use the C++ REST SDK, it is possible that some C++ code will be pulled in via header includes. To avoid errors later, it is easiest to simply rename all your project sources to use '.mm'.