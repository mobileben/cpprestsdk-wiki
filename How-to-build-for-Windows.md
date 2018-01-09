# How to setup, build, and run tests on Windows

## Setup [Git](http://git-scm.com/)

If you use PowerShell, we recommend installing Posh-Git ([here](Posh-git) is how).  

If you use cmd.exe, open the **Developer Command Prompt for VS2017** ([here](Setting-up-Visual-Studio-command-line-prompt) is how)  

## Get the source

```bash
git clone https://github.com/Microsoft/cpprestsdk.git
```

## Build from source

We recommend using CMake and [Vcpkg](https://github.com/microsoft/vcpkg) to build from source on Windows.

First, install the dependencies for the platform you are interested in:
```bash
vcpkg install --triplet x64-windows boost-system zlib openssl boost-date-time boost-regex websocketpp
```
Then, configure CMake against the `Release\CMakeLists.txt` file in the normal way while following the vcpkg instructions (Note: If you use the `cmake-gui.exe` tool, make sure to select the "Cross-compiling" option and choose the Vcpkg toolchain):
```
cpprestsdk> mkdir build.x64v141
cpprestsdk> cd build.x64v141
cpprestsdk/build.x64v141> cmake ../Release -A x64 -DCMAKE_TOOLCHAIN_FILE=/path/to/vcpkg/scripts/buildsystems/vcpkg.cmake
```

You can either run `cmake --build . --config Release` or open the generated solution file in Visual Studio. The built binaries will be in `Binaries\Release` or `Binaries\Debug` depending on the configuration you build with.