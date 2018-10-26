# How to setup, build, and run tests on Linux

0\. We're in apt-get! Use `sudo apt-get install libcpprest-dev` to get all our development files. If your distribution has an outdated version, continue on to build from source.

1\. Install [Ubuntu](http://www.ubuntu.com/download) 16.04 or later.  

2\. Install all the needed build tools and libraries  

```bash
sudo apt-get install g++ git libboost-atomic-dev libboost-thread-dev libboost-system-dev libboost-date-time-dev libboost-regex-dev libboost-filesystem-dev libboost-random-dev libboost-chrono-dev libboost-serialization-dev libwebsocketpp-dev openssl libssl-dev ninja-build
```

Minimum versions:
- g++: 4.8
- libboost: 1.58
- libssl: 1.0.0
- cmake: 3.1

3\. Clone the project using Git (it will be stored in the folder "casablanca"):  

```bash
git clone https://github.com/Microsoft/cpprestsdk.git casablanca
```

Going forward, you will want to pull from the _master_ branch, which will always contain the last known release.  

4\. Build the SDK in Debug mode

```bash
cd casablanca
mkdir build.debug
cd build.debug
cmake .. -DCMAKE_BUILD_TYPE=Debug
make
```

You can build the Release version by specifying -DCMAKE_BUILD_TYPE=Release on the cmake line instead.  
You can also build the static libraries instead of the shared libraries by adding -DBUILD_SHARED_LIBS=0 on the cmake line above.

5\. After building you can run the tests by executing the test_runner inside the "Binaries" folder:  

```bash
cd Release/Binaries
./test_runner *_test.so
```

6\. To install on your system run: sudo make install

7\. You can check your first program by compiling it using necessary command line arguments 
```bash
g++ -std=c++11 my_file.cpp -o my_file -lboost_system -lcrypto -lssl -lcpprest
./my_file
```
_For versions prior to 2.0, please visit [Setup and Build on Linux (1.4)](https://casablanca.codeplex.com/wikipage?title=Setup%20and%20Build%20on%20Linux%20%281.4%29&referringTitle=Setup%20and%20Build%20on%20Linux)_