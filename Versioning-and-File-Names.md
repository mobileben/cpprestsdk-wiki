# Versioning and File Names

The C++ REST SDK uses the following naming convention for DLL, LIB and PDB files:

`<name><vs_version>[os_flavor]_<library_version>.<extension>`

*   `<name>`: The name of the file, for now is **cpprest** .
*   `<vs_version>`: The name of the version of Visual Studio, **110** (Visual Studio 2012) or **120** (Visual Studio 2013).
*   `[os_flavor]` (optional): The flavor of the targeted operating system or runtime, such as **d_xp** for Debug in XP and **_app** for Release in winrt (note leading underscore).
*   `<library_version>`: the underscore-separated library version major and minor values, such as 2_0\. The C++ REST SDK library version is a triplet of {major , minor, revision}.
    *   The revision changes are for small incremental fixes not requiring binary and source breaking changes. The revision therefore does not contribute to the library name.
    *   The minor version change might require a source breaking change and almost certainly a binary breaking change.
    *   The major version change occurs as a result of the addition (or deprecation) of major features and API sets.
*   `<extension>`: one of **dll**, **lib** or **pdb**.
