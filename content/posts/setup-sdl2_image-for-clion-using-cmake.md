+++
title = 'Setup SDL2_image for CLion using CMake'
date = 2021-12-01
draft = false
tags = ["cpp", "sdl2_image", "clion", "cmake"]
+++

## Setup SDL2_image for CLION using CMAKE

Guide for setting up SDL2_image for CLION using CMAKE

Since setting up SDL2_IMAGE is very similar to setting up SDL2, most of this guide will be very similar to the [setup SDL2 for CLION](https://blog.ahmadz.ai/sdl2-for-clion-and-cmake/) guide.

Open your project and set the default toolchain (found in settings) to the bundled mingw that comes with CLion.

![CLion’s Toolchains window](https://cdn-images-1.medium.com/max/2000/0*Pv3cW9hg0ZuxxY1k.png)

Create a cmake/modules directory in your project root.

![directory structure](https://cdn-images-1.medium.com/max/2000/1*WbAHOJFxwBsrjTbtU99EOQ.png)

Put the following cmake file in the newly created cmake/modules directory
{{< gist 416rehman 90cad642bc21b07e1d51aa8976502024 findsdl2_image-cmake >}}

Download the SDL2_image development library for mingw from [https://github.com/libsdl-org/SDL_image/releases/.](https://github.com/libsdl-org/SDL_image/releases/) After downloading, unzip the folder in your project root directory.

![Make sure to get the development package](https://cdn-images-1.medium.com/max/2410/1*dZXz-xDxTX2ZdspwYkfueA.png)

Add the following above the add_executable statement in your CMakeLists.txt
```cmake
    set(SDL2_IMAGE_PATH "SDL2_image-2.6.2\\x86_64-w64-mingw32") 
    find_package(SDL2_image REQUIRED) 
    include_directories(${SDL2_IMAGE_INCLUDE_DIRS})
```

Then link the SDL2_image libraries by putting the following line after the add_executable statement
```cmake
    # Link SDL2 and SDL2_image 
    target_link_libraries(breakout ${SDL2_LIBRARY} ${SDL2_IMAGE_LIBRARIES})
```

Note: The code snippet above also links the SDL2_LIBRARY which is part of the base SDL2 library.

Finally, your CMakeLists.txt should look like this, where breakout is the name of your project.
```cmake
    cmake_minimum_required(VERSION 3.23) 
    project(breakout) 
    set(CMAKE_CXX_STANDARD 23) 
    set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules) 
    
    # SDL2 Guide @ https://blog.ahmadz.ai/sdl2-for-clion-and-cmake/ 
    set(SDL2_PATH "SDL2-2.26.0\\x86_64-w64-mingw32") 
    find_package(SDL2 REQUIRED) 
    include_directories(${SDL2_INCLUDE_DIR})
     
    # SDL2_image 
    set(SDL2_IMAGE_PATH "SDL2_image-2.6.2\\x86_64-w64-mingw32") 
    find_package(SDL2_image REQUIRED) 
    include_directories(${SDL2_IMAGE_INCLUDE_DIRS}) 
    add_executable(breakout main.cpp) 
    
    # Link SDL2 and SDL2_image 
    target_link_libraries(breakout ${SDL2_LIBRARY} ${SDL2_IMAGE_LIBRARIES})
```

That should be it, try using the #include <SDL_image.h> statement in your source code, everything should work.
