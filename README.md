![Check](https://github.com/TheLartians/CPMLicenses.cmake/workflows/Check/badge.svg)
![Check](https://github.com/TheLartians/CPMLicenses.cmake/workflows/Style/badge.svg)

# CPMLicenses.cmake

A license collector for your projects using CPM.cmake.

## About

Many open-source projects require adding a disclaimer to your binary reproducing the dependencies license. Unfortunately, finding and maintaining the disclaimers for all dependencies can be a tedious task. Luckily, if you're using [CPM.cmake](https://github.com/TheLartians/CPM.cmake) to add the dependencies, this process can be automated using this package.

CPMlicenses.cmake will automatically scan all source directories of your CPM.cmake dependencies and find any file that begins with `LICENCE` or `LICENSE`, appending the contents to an output file that you can use as a license disclaimer. If no license has been found for a package, a warning will be emitted. 

## Usage

Use [CPM.cmake](https://github.com/TheLartians/CPM.cmake) to add CPMLicenses.cmake to your project.

```cmake
CPMAddPackage(
  NAME CPMLicenses.cmake 
  GITHUB_REPOSITORY cpm-cmake/CPMLicenses.cmake
  VERSION 0.0.5
)
```

After all additional dependencies have been added, create the target by calling `cpm_licenses_create_disclaimer_target(<target name> <output file> <packages>)`. Afterwards, simply build the CMake target with the specified name to create the file containing the licenses.

For example, with the configuration

```cmake
cpm_licenses_create_disclaimer_target(
  write-licenses "${CMAKE_CURRENT_BINARY_DIR}/third_party.txt" "${CPM_PACKAGES}"
)
```

building the target `write licenses` (e.g. calling `cmake --build build --target write-licenses`) will create the file `third_party.txt` in your build directory, containing all license disclaimers of dependencies added by CPM.cmake.

## Example output

The generated disclaimer contains the name of the dependency as well as the included license text. Below is an example of the disclaimer generated for [Catch2](https://github.com/catchorg/Catch2). 

```
The following software may be included in this product: Catch2. This software contains the following license and notice below:

Boost Software License - Version 1.0 - August 17th, 2003

Permission is hereby granted, free of charge, to any person or organization
obtaining a copy of the software and accompanying documentation covered by
this license (the "Software") to use, reproduce, display, distribute,
execute, and transmit the Software, and to prepare derivative works of the
Software, and to permit third-parties to whom the Software is furnished to
do so, all subject to the following:

The copyright notices in the Software and this entire statement, including
the above license grant, this restriction and the following disclaimer,
must be included in all copies of the Software, in whole or in part, and
all derivative works of the Software, unless such copies or derivative
works are solely in the form of machine-executable object code generated by
a source language processor.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT. IN NO EVENT
SHALL THE COPYRIGHT HOLDERS OR ANYONE DISTRIBUTING THE SOFTWARE BE LIABLE
FOR ANY DAMAGES OR OTHER LIABILITY, WHETHER IN CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
DEALINGS IN THE SOFTWARE.
```
