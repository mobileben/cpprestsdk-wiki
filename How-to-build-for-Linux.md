# How to setup, build, and run tests on Linux

1\. Install [Ubuntu](http://www.ubuntu.com/download) 14.04 or later.  

2\. Install all the needed build tools and libraries  

<pre>sudo apt-get install g++-4.8 g++ git make libboost1.54-all-dev libssl-dev cmake
</pre>

3\. Clone the project using Git (it will be stored in the folder "casablanca"):  

<pre>git clone https://github.com/Microsoft/cpprestsdk.git casablanca
</pre>

Going forward, you will want to pull from the _master_ branch, which will always contain the last known release.  

4\. Build the SDK for Release  

<pre>cd casablanca/Release
mkdir build.release
cd build.release
CXX=g++-4.8 cmake .. -DCMAKE_BUILD_TYPE=Release
make
</pre>

You can build the Debug version by specifying -DCMAKE_BUILD_TYPE=Debug on the cmake line instead.  

5\. After building you can run the tests by executing the ./run_tests.sh script inside the "Binaries" folder:  

<pre>cd Binaries
./test_runner *_test.so
</pre>

_For versions prior to 2.0, please visit [Setup and Build on Linux (1.4)](https://casablanca.codeplex.com/wikipage?title=Setup%20and%20Build%20on%20Linux%20%281.4%29&referringTitle=Setup%20and%20Build%20on%20Linux)_