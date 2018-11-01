# How to setup, build, and run tests on OSX

1\. Ensure you have the most recent version of OSX and Xcode (As of this writing, 10.13.x and 10.x respectively).  

2\. You will need to install the development files for Boost and OpenSSL. Our recommendation is using the Homebrew package manager:  

[http://brew.sh/](http://brew.sh/)  

3\. If you are using the Homebrew package manager, install the required development tools:  

```
brew install cmake git boost openssl ninja
```

4\. Clone the project using Git (it will be stored in the folder "casablanca"):  

```
git clone https://github.com/Microsoft/cpprestsdk.git casablanca
cd casablanca
git submodule update --init
```

Going forward, you will want to pull from the _master_ branch, which will always contain the last known release.  

5\. Build the SDK in Debug mode  

```
mkdir build.debug
cd build.debug
cmake ../Release -DCMAKE_BUILD_TYPE=Debug
ninja
```

You can build the Release version by replacing every instance of Debug with Release in the previous directions.  

6\. After building you can run the tests by launching the <span class="codeInline">test_runner</span> executable:  

```
cd Binaries
./test_runner *_test.dylib
```