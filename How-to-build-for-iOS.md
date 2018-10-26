# How to build and setup on iOS

First, follow all the instructions on [Setup and Build on OSX](How-to-build-for-Mac-OS-X). While some steps could be skipped if you _only_ want to build for iOS, it is useful to diagnose problems during the initial iOS project setup by replicating it on OSX.  

The C++ REST SDK depends on Boost and OpenSSL when used on iOS. It is a non-trivial task to cross-compile libraries for iOS, however there are scripts available online with nonrestrictive licenses to compile many popular libraries -- among these libraries are Boost and OpenSSL.  

This document will walk through the steps to build the C++ REST SDK and its dependencies into a form suitable for use with iOS applications.  

For this walkthrough, we assume you are working within the `Build_iOS` directory of the project.  

```
git clone https://github.com/Microsoft/cpprestsdk.git casablanca
pushd casablanca/Build_iOS
./configure.sh
```

This will produce a universal static library called "libcpprest.a" for the x86_64 simulator, and arm64.  

## Using the C++ REST SDK

You will need to link against the following from your project:  

*   build.debug/libcpprest.a
*   boost.framework
*   openssl/lib/libcrypto.a
*   openssl/lib/libssl.a
*   libiconv.dylib (Available within the default list of libraries to link)
*   Security.framework (Available within the default list of libraries to link)

You will also need to add the following paths as additional include directories:  

*   ../Debug/include
*   boost.framework/Headers
*   openssl/include

This should allow you to reference and use the C++ REST SDK from your C++ and Objective-C++ source files. Note: you should change all .m files in your project to .mm files, because even if the source file itself does not use the C++ REST SDK, it is possible that some C++ code will be pulled in via header includes. To avoid errors later, it is easiest to simply rename all your project sources to use '.mm'.