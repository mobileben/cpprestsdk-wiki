# How to use the C++ REST SDK on Android (2.3+)

The easiest way to use the C++ REST SDK on android is to add the "C++ REST SDK - Android" NuGet package available via Visual Studio 2015 (including the free Community Edition). Go to [Using NuGet to add the C++ REST SDK to a VS project](How-to-use-the-C---Rest-SDK-NuGet-package) for general information about adding NuGet packages.  

After adding the package, you will need to enable exceptions, rtti, c++11, and the GNU standard library (gnustl_static). These options are all found under <span class="codeInline">Configuration Properties</span> as follows:  

*   General -> Use of STL = gnustl_static
*   C++ -> Code Generation -> Enable C++ Exceptions = Unwind Tables (-funwind-tables)
*   C++ -> Language -> Enable Run-Time Type Information = Yes
*   C++ -> Language -> C++ Language Standard = C++11

Finally, you will need to add some small initialization code detailed under [StaticLinking](#StaticLinking). In addition to the instructions mentioned here, you can get a walkthrough with a simple sample application from this [blog post](http://blogs.msdn.com/b/vcblog/archive/2015/01/06/targeting-android-with-the-c-rest-sdk.aspx).  

## Non-NuGet

First, follow the steps here: [Setup and Build on Android](https://casablanca.codeplex.com/wikipage?title=Setup%20and%20Build%20on%20Android&referringTitle=Use%20on%20Android)  

You will need to link against the following from your project:  

*   build.armv7.debug/Binaries/libcpprest.a
*   Boost-for-Android/build/lib/libboost_*-clang-mt-1_55.a
*   libiconv/armeabi-v7a/lib/libiconv.a (removed in version 2.6.0)
*   openssl/armeabi-v7a/lib/libssl.a
*   openssl/armeabi-v7a/lib/libcrypto.a
*   libm (math library, link with -lm)
*   liblog (logging library, link with -llog)

You will also need to include the headers for boost and the C++ REST SDK:  

*   ../../Release/include
*   Boost-for-Android/build/include/boost-1_55

Use the included documentation with your NDK to add these prebuilt libraries and include paths. An outdated version of the relevant documentation is hosted at [http://www.kandroid.org/ndk/docs/PREBUILTS.html](http://www.kandroid.org/ndk/docs/PREBUILTS.html).  

<a name="StaticLinking"></a>

## Static Linking and Initialization

The C++ REST SDK prefers static linking for Android since version 2.3\. This makes it much simpler for both Native Activities and Java-based Activities since there is only one shared object file in the final app bundle.  

However, in order to prevent conflicts with other code, you must provide a pointer to the JVM to the SDK before it can initialize. This pointer is provided through the method <span class="codeInline">cpprest_init(vm)</span> which is located in the <span class="codeInline">pplx/pplxtasks.h</span> header file.  

**Note: If you do not do this, your application will terminate upon attempting to create a task.**  

### Native Activity (android_main)

For most NativeActivity projects, if you have an entry point like  

```c++
void android_main(struct android_app* state) {
```

You can add the following code at the top of the <span class="codeInline">android_main</span> function:  

```c++
cpprest_init(state->activity->vm);
```

### Other Shared Libraries (JNI_OnLoad)

For most shared library projects, you will simply need to copy the following code into your main cpp file:  

```c++
extern "C" jint JNI_OnLoad(JavaVM* vm, void* reserved)
{
    JNIEnv* env;
    if (vm->GetEnv(reinterpret_cast<void**>(&env), JNI_VERSION_1_6) != JNI_OK)
    {
        return -1;
    }

    cpprest_init(vm);
    return JNI_VERSION_1_6;
}
```