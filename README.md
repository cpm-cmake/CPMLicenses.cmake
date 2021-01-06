# CPMLicenses.cmake

A license collector for your projects using CPM.cmake.

## About

Many open-source projects require adding a disclaimer to your binary reproducing the dependencies license. Finding and updating the disclaimer for all dependencies can be a tedious task. Luckily, if you've used [CPM.cmake](https://github.com/TheLartians/CPM.cmake) to add the dependencies this process can be automated.

CPMlicenses.cmake will automatically scan all source directories of your CPM.cmake dependencies and find any file that begins with `LICENCE` or `LICENSE`, appending the contents to an output file that you can use as a license disclaimer. If no license has been found for a package, a warning will be emitted. 

## Usage

Use [CPM.cmake](https://github.com/TheLartians/CPM.cmake) to add CPMLicenses.cmake to your project.

```cmake
CPMAddPackage(
  NAME CPMLicenses.cmake 
  GITHUB_REPOSITORY TheLartians/CPMLicenses.cmake
  VERSION 0.0.2
)
```

After all additional dependencies have been added, create the target by calling `cpm_licenses_create_disclaimer_target(<target name> <output file> <packages>)`. Afterwards, simply build the CMake target with the specified name to create the file containing the licenses.

For example, with the configuration

```cmake
cpm_licenses_create_disclaimer_target(
  write-licenses "${CMAKE_CURRENT_BINARY_DIR}/third_party.txt" "${CPM_PACKAGES}"
)
```

building the target `write licenses` (e.g. `cmake --build build --target write-licenses`) will create the file `build/third_party.txt` containing all license disclaimers of the dependencies.
