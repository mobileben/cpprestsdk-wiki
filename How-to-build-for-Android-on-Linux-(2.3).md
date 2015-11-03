# How to build and setup for Android on Linux (2.3+)

**Note: if you are on Windows, we now have a NuGet package! Go straight to [Use on Android](How-to-build-for-Android) to get started.**  

First, follow all the instructions on [Setup and Build on Linux](How-to-build-for-Linux). While some steps could be skipped if you _only_ want to build for Android, it is useful to diagnose problems with a desktop build first.  

Second, while our dependencies require a Linux system to build, once they are built you can transfer the products to a Windows computer and use them with your existing Android project under Eclipse. More information on this is provided in the relevant step. The C++ REST SDK depends on Boost, libiconv, and OpenSSL when used on Android. The script below will compile all of these dependencies for you.  

For this walkthrough, we assume you are working within the <span class="codeInline">Build_Android</span> directory of the C++ REST SDK.  

<pre>git clone https://git01.codeplex.com/casablanca
cd casablanca/Build_android
</pre>

## Install the Android NDK

The following instructions are written and tested against the android NDK version r10\. Future and past versions may or may not work. The android NDK is available at [http://developer.android.com/tools/sdk/ndk/index.html](http://developer.android.com/tools/sdk/ndk/index.html). If version r10 is not available for download, hard links are provided below.  

*   Hard link to version r10 for Linux: [http://dl.google.com/android/ndk/android-ndk32-r10-linux-x86_64.tar.bz2](http://dl.google.com/android/ndk/android-ndk32-r10-linux-x86_64.tar.bz2)
*   Hard link to version r10 for Windows: [http://dl.google.com/android/ndk/android-ndk32-r10-windows-x86_64.zip](http://dl.google.com/android/ndk/android-ndk32-r10-windows-x86_64.zip)

For this tutorial, we assume it is installed at <span class="codeInline">~/android-ndk-r10/</span>.  

## Building the C++ REST SDK

<pre>mkdir build
cd build
../configure.sh --ndk ~/android-ndk-r10
</pre>

This will build armv7 binaries under the <span class="codeInline">build.armv7.debug/Binaries</span> and <span class="codeInline">build.armv7.release/Binaries</span> folders.  

## Using the C++ REST SDK

See [Use on Android](How-to-build-for-Android).  

## Running Tests

If you want to run the tests for Android we have a test runner project that can used. All the tests can be built by running msbuild from the command line. For example the following setups up the Visual Studio tools and builds x86 Debug for the simulator.  

<pre>.\setup_ps_env_VS2015.ps1
cd Release
msbuild /p:Platform=x86 /p:Configuration=Debug
</pre>

Once the build completes open the following androidproj file in Visual Studio, which packages the tests.  

<pre>Release\tests\common\TestRunner\vs14.android\TestRunner.android.Packaging\TestRunner.android.Packaging.androidproj
</pre>

Make sure the configuration is x86 Debug and then start debugging with F5\. This will launch the Android emulator and start running the tests. Output of the tests goes to Logcat, which can be brought up the Visual Studio menu with Tools->Android Tools->Logcat. Look for messages with the tag 'UnitTestpp', in particular at the end of running the tests there are several '--- Flush buffer ---' messages in a row.  

## Old versions

For reference, the older documentation is available at [Setup and Build on Android 2.2](https://casablanca.codeplex.com/wikipage?title=Setup%20and%20Build%20on%20Android%202.2&referringTitle=Setup%20and%20Build%20on%20Android).