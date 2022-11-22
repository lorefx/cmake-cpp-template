# cmake-cpp-template
CMake / C++ project template with support for Static Analysisi Sanitizers and multiple Preset targets
<br>
<br>
<br>
# More complex example - Test
This is a CMakeFile.txt scenario where we want to apply the preset settings only to our code, leaving the imported libraries to do their own compilation according to their CMakeFile.

```
# Add your own libraries into LoadExtLibrary.cmake. Uses FetchContent
include(${CMAKE_SOURCE_DIR}/cmake/LoadExtLibrary.cmake)
load_gtest()

# Dependencies Library - not compiled with preset settings
add_library(mytestproject_deps INTERFACE)

target_link_libraries(mytestproject_deps
  INTERFACE 
    gtest_main
)

# Tests executable - compiled with preset settings
add_executable(mytestproject)

target_link_libraries(mytestproject
  PRIVATE 
    mytestproject_deps
)

# ApplyPreset is used to apply preset settings only to specific targets
include(${CMAKE_SOURCE_DIR}/cmake/ApplyPreset.cmake)
apply_preset_to(mytestproject)

# Testing Enable
enable_testing()
include(GoogleTest)
gtest_discover_tests(mytestproject)
```