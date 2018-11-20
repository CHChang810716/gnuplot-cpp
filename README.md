# gnuplot-cpp
Fork form https://code.google.com/archive/p/gnuplot-cpp and add CMake build scripts.

# Prerequisite

* CMake >= 3.10
* Gnu plot ( should visable in system path )

# Usage

## Install gnuplot-cpp
```
git clone git@github.com:CHChang810716/gnuplot-cpp.git
cd gnuplot-cpp
mkdir build
cd build
cmake ..
cmake --build . --target install
```

## Use gnuplot-cpp in your project
```
cmake_minimum_required (VERSION 3.10)
project (use-GnuPlotCpp) # Project Name
find_package(GnuPlotCpp CONFIG REQUIRED)
add_executable(your_exe your_exe.cpp)
target_link_libraries(your_exe GnuPlotCpp::gnuplot_i)
```
```
cmake . -DGnuPlotCpp_DIR=<gnu plot cpp install path>/lib/cmake/GnuPlotCpp
```
