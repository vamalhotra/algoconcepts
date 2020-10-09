---
layout: post
---
# Using CMake to generate Visual studio project in windows

A project using cmake will come with CMakeLists.txt script. This script defines targets where each target represents an executable or library or some other output for the build pipeline. On windows, we can use CMakeLists.txt to generate sln/vcproj files which can then be opened in visual studio and compiled/debugged in regular manner.

If you have never used cmake, first step is to download cmake binaries from https://cmake.org/download/

Step 2 is to open cmd and cd to folder containing CMakeLists.txt.

Step 3 is to generate sln/vcproj files by running following commands:

#Create build directory

mkdir build

cd build

#Generate sln file for a target version of visual studios

#list all available targets.

cmake --help 

#generate files for a selected target. Last .. points to parent directory where CMakeLists.txt exists.

cmake -G "Visual Studio 15 2017 Win64" ..



Alternatively, above commands can be replaced with:

mkdir build
cmake -H. -Bbuild



References:

1. [https://preshing.com/20170511/how-to-build-a-cmake-based-project/#running-cmake-from-the-command-line](https://preshing.com/20170511/how-to-build-a-cmake-based-project/#running-cmake-from-the-command-line)