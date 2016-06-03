# How to setup, build, and run tests on OSX

1\. Ensure you have the most recent version of OSX and Xcode (As of this writing, 10.9.2 and 5.0.2 respectively).  

2\. You will need to install the development files for Boost and OpenSSL. Our recommendation is using the Homebrew package manager:  

[http://brew.sh/](http://brew.sh/)  

3\. If you are using the Homebrew package manager, install the required development tools:  

```
brew install cmake git openssl boost libiconv
```

4\. Clone the project using Git (it will be stored in the folder "casablanca"):  

```
git clone https://github.com/Microsoft/cpprestsdk.git casablanca
```

Going forward, you will want to pull from the _master_ branch, which will always contain the last known release.  

5\. Build the SDK in Debug mode  

```
cd casablanca
mkdir build.debug
cd build.debug
cmake ../Release -DCMAKE_BUILD_TYPE=Debug
make -j 4
```

You can build the Release version by replacing every instance of Debug with Release in the previous directions.  

***
**XVELA NOTES:**
This fork fixes five compile errors in the original source code when you perform the `make -j 4` step.
These all are due to calls to `std::move`. Commenting out the move call seems to solve the problem.
You may also need to run `brew link openssl --force` but I do not recall what prompted me to do that.
***

6\. After building you can run the tests by launching the <span class="codeInline">test_runner</span> executable:  

```
cd Binaries
./test_runner *_test.dylib
```