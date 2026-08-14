```
cmake_minimum_required(VERSION 3.10)

project(PersonExample VERSION 1.0)

set(CMAKE_CXX_STANDARD 11)

set(CMAKE_CXX_STANDARD_REQUIRED True)

file(GLOB SOURCES "src/*.cpp")

add_executable(test ${SOURCES})
```