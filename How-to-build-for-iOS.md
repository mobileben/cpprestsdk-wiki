# How to build and setup on iOS

First, follow all the instructions on [Setup and Build on OSX](How-to-build-for-Mac-OS-X). While some steps could be skipped if you _only_ want to build for iOS, it is useful to diagnose problems during the initial iOS project setup by replicating it on OSX.  

The C++ REST SDK depends on Boost and OpenSSL when used on iOS. It is a non-trivial task to cross-compile libraries for iOS, however there are scripts available online with nonrestrictive licenses to compile many popular libraries -- among these libraries are Boost and OpenSSL.  

This document will walk through the steps to build the C++ REST SDK and its dependencies into a form suitable for use with iOS applications.  

For this walkthrough, we assume you are working within the <span class="codeInline">Build_iOS</span> directory of the project.  

```
git clone https://github.com/Microsoft/cpprestsdk.git casablanca
pushd casablanca/Build_iOS
```

## Building OpenSSL

To build OpenSSL, use the script provided by the OpenSSL-for-iPhone project.  

```
git clone --depth=1 https://github.com/x2on/OpenSSL-for-iPhone.git
pushd OpenSSL-for-iPhone
./build-libssl.sh
popd
```

After building the library, move the include files and libraries to <span class="codeInline">Build_iOS/openssl/include</span> and <span class="codeInline">Build_iOS/openssl/lib</span> respectively.  

```
mkdir openssl
mv OpenSSL-for-iPhone/include openssl
mv OpenSSL-for-iPhone/lib openssl
```

This completes building OpenSSL.  

project link: [https://github.com/x2on/OpenSSL-for-iPhone](https://github.com/x2on/OpenSSL-for-iPhone)  

## Building Boost

To build Boost, use the script provided by the boostoniphone project. The main project author seems to have not continued maintaining the project, however there are a few actively maintained forks. We now recommend using the following repository from faithfracture:  

```
git clone https://gist.github.com/c629ae4c7168216a9856.git boostoniphone
pushd boostoniphone
```

The script `boost.sh` provided by the boostoniphone project has a variable at the top of the file to specify which parts of boost need be compiled. This variable must be changed to include the parts needed for the C++ REST SDK: chrono, filesystem, random, regex, system, thread. This can easily be done by applying our patch.  

```
git apply ../fix_boost_building_script.patch
chmod +x boost.sh
./boost.sh
```

The headers need to be moved to allow inclusion via `"boost/foo.h"`.  

```
pushd ios/framework/boost.framework/Versions/A
mkdir Headers2
mv Headers Headers2/boost
mv Headers2 Headers
popd
```

Finally, the product framework must be moved into place.  

```
popd
mv boostoniphone/ios/framework/boost.framework .
```

This completes building Boost.  

## Preparing the C++ REST SDK build

The C++ REST SDK uses CMake for cross-platform compatibility. To build on iOS, we specifically use the toolchain file provided by a ios-cmake fork on GitHub. Also, you need to use the Clang compiler. This can easily be done by applying our patch. 

```
git clone https://github.com/cristeab/ios-cmake.git
pushd ios-cmake
git apply ../fix_ios_cmake_compiler.patch
popd
```

This completes the preparation for building Casablanca.  

project link: [https://github.com/cristeab/ios-cmake](https://github.com/cristeab/ios-cmake)  

## Building the C++ REST SDK

Now we are ready to build the C++ REST SDK for iOS. Invoke the ios-buildscripts subproject in the usual CMake fashion:  

```
mkdir build.ios
pushd build.ios
cmake .. -DCMAKE_BUILD_TYPE=Release
make
popd
```

This will produce a universal static library called "libcpprest.a" inside the 'build.ios' directory for the i386 simulator, x86_64 simulator, armv7, armv7s, and arm64.  

## Running Tests

If you want to run the tests in the iOS simulator you can simply execute 'make test'.  

```
pushd build.ios
make test
popd
```

This will go and run all the tests in the iOS simulator, but the output of the tests won't be displayed. If you want to run the tests directly and see the full output you can manually execute the tests directly with xcodebuild.  

```
pushd Release/tests/common/TestRunner/ios
xcodebuild test -project ios_runner.xcodeproj -configuration=Release -scheme ios_runner -destination "platform=iOS Simulator,name=iPhone 6"
```

Now you will see the output of each of the tests cases as they run.  

## Using the C++ REST SDK

You will need to link against the following from your project:  

*   build.ios/libcpprest.a
*   boost.framework
*   openssl/lib/libcrypto.a
*   openssl/lib/libssl.a
*   libiconv.dylib (Available within the default list of libraries to link)
*   Security.framework (Available within the default list of libraries to link)

You will also need to add the following paths as additional include directories:  

*   ../Release/include
*   boost.framework/Headers
*   openssl/include

This should allow you to reference and use the C++ REST SDK from your C++ and Objective-C++ source files. Note: you should change all .m files in your project to .mm files, because even if the source file itself does not use the C++ REST SDK, it is possible that some C++ code will be pulled in via header includes. To avoid errors later, it is easiest to simply rename all your project sources to use '.mm'.