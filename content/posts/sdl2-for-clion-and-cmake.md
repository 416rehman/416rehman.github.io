+++
title = 'Setup SDL2 for CLion using CMake'
date = 2021-12-29T09:38:40-06:00
draft = false
tags = ["cpp", "sdl2", "clion", "cmake"]
+++

## Setup SDL2 for CLion using CMake

Setting up SDL2 with CMake for CLion can be a hassle but in this tutorial we are going to solve that.

First, open your project and set the default toolchain (found in settings) to the bundled mingw that comes with CLion.

![](https://cdn-images-1.medium.com/max/2000/0*2YKi2WrZbhGLhFaV.png)

Create a cmake/modules directory in your project root.

![directory structure](https://cdn-images-1.medium.com/max/2000/1*bcn__ZCUE7_TIwxfrfRSHg.png)

Put the following cmake file in the newly created cmake/modules directory

{{< gist 416rehman 4b5a88f5e5f033dfe8f7abd7bb768bde gistfile1.txt >}}

Download the SDL2 development library for mingw from [https://github.com/libsdl-org/SDL/releases/tag/release-2.26.0 (](https://github.com/libsdl-org/SDL/releases/tag/release-2.26.0)SDL2-devel-2.26.0-mingw.zip). After downloading, unzip the folder in your project root, the directory should look like this after unzipping it.

![directory structure after getting the SDL package](https://cdn-images-1.medium.com/max/2000/1*kfCQgQmGiuo63t41s3SijA.png)

**Optional**: You can mark the SDL2-2.0.18 folder as excluded so CLion doesn't have to index it as part of your project's source code by right clicking the SDL2-2.0.18 > Mark Directory As > Excluded.

![](https://cdn-images-1.medium.com/max/2000/1*NVBDCpYjCt2If_XkTItDfg.png)

Add the following above the add_executable statement in your CMakeLists.txt

```cmake
    set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules) 
    set(SDL2_PATH "SDL2-2.0.18\\x86_64-w64-mingw32") 
    find_package(SDL2 REQUIRED) 
    include_directories(${SDL2_INCLUDE_DIR})
```

Add the following below the add_executable statement in your CMakeLists.txt
```cmake
    target_link_libraries(cppGameDev ${SDL2_LIBRARY})
```

Your final CMakeLists.txt file should look like this, where cppGameDev is the name of your project
```cmake
    cmake_minimum_required(VERSION 3.16) 
    project(cppGameDev) 
    set(CMAKE_CXX_STANDARD 14) 
    
    set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules) 
    set(SDL2_PATH "SDL2-2.0.18\\x86_64-w64-mingw32") 
    find_package(SDL2 REQUIRED) 
    include_directories(${SDL2_INCLUDE_DIR}) 
    
    add_executable(cppGameDev main.cpp game/Game.cpp game/Game.h) 
    target_link_libraries(cppGameDev ${SDL2_LIBRARY})
```

Finally, copy the SDL2-2.0.18\x86_64-w64-mingw32\bin\SDL2.dll file to cmake-build-debug and or cmake-build-release folders like this

![SDL2.dll is moved to the same directory as the compiled binary so it can be found by the binary.](https://cdn-images-1.medium.com/max/2000/1*DJs1Hg3oORFWzNRRlO1e1A.png)

To test that everything works fine and SDL2 is setup, I recommend following this LazyFoo tutorial to get SDL2 started and running: [https://lazyfoo.net/tutorials/SDL/01_hello_SDL/index2.php](https://lazyfoo.net/tutorials/SDL/01_hello_SDL/index2.php)