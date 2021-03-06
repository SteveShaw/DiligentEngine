# DiligentEngine

Diligent Engine is a light-weight cross-platform abstraction layer between the application and the platform-specific graphics API. 
Its main goal is to take advantages of the next-generation APIs such as Direct3D12 and Vulkan, but at the same time provide support 
for older platforms via Direct3D11, OpenGL and OpenGLES. Diligent Engine exposes common front-end for all supported platforms and 
provides [interoperability with underlying native API](http://diligentgraphics.com/diligent-engine/native-api-interoperability/). 
It also supports [integration with Unity](http://diligentgraphics.com/diligent-engine/unity-integration/) and is designed to be used 
as a graphics subsystem in a standalone game engine, Unity native plugin or any other 3D application. It is distributed under 
[Apache 2.0 license](License.txt) and is free to use. The engine contains 
[shader source code converter](http://diligentgraphics.com/diligent-engine/shader-converter/) that allows shaders authored in HLSL to 
be translated to GLSL.

## Supported Plaforms and Low-Level Graphics APIs

| Platform                   | APIs                                |
| -------------------------- | ----------------------------------- |
| Win32 (Windows desktop)    | Direct3D11, Direct3D12, OpenGL4.2+  |
| Universal Windows Platform | Direct3D11, Direct3D12              |
| Android                    | OpenGLES3.0+                        |
| Linux                      | OpenGL4.2+                          |

## Build Status

| Platform                   | Status        |
| -------------------------- | ------------- |
| Win32/Universal Windows    | [![Build Status](https://ci.appveyor.com/api/projects/status/github/DiligentGraphics/DiligentEngine?svg=true)](https://ci.appveyor.com/project/DiligentGraphics/diligentengine) |
| Linux                      | [![Build Status](https://travis-ci.org/DiligentGraphics/DiligentEngine.svg?branch=master)](https://travis-ci.org/DiligentGraphics/DiligentEngine)      |


Last Stable Release - [v2.1.b](https://github.com/DiligentGraphics/DiligentEngine/tree/v2.1.b)

# Clonning the Repository

This is the master repository that contains three [submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules). To get the repository and all submodules, use the following command:

 git clone --recursive https://github.com/DiligentGraphics/DiligentEngine.git 
 
 Alternatively, you can get master repository fisrt, and then individually clone all submodules into the engine's root folder.
 
 To checkout the last stable release, run the following commands:
 
* git checkout tags/v2.1.b

* git submodule update --init --recursive


## Repository Structure

Master repository includes the following submodules:

* [Core](https://github.com/DiligentGraphics/DiligentCore) submodule provides basic engine functionality. 
  It implements the engine API using 
  [Direct3D11](https://github.com/DiligentGraphics/DiligentCore/tree/master/Graphics/GraphicsEngineD3D11), 
  [Direct3D12](https://github.com/DiligentGraphics/DiligentCore/tree/master/Graphics/GraphicsEngineD3D12), and
  [OpenGL/GLES](https://github.com/DiligentGraphics/DiligentCore/tree/master/Graphics/GraphicsEngineOpenGL). 
  It also implements 
  [HLSL to GLSL source code converter](https://github.com/DiligentGraphics/DiligentCore/tree/master/Graphics/HLSL2GLSLConverterLib).
  The module is self-contained and can be built by its own.
* [Tools](https://github.com/DiligentGraphics/DiligentTools) submodule contains 
  [texture loading library](https://github.com/DiligentGraphics/DiligentTools/tree/master/TextureLoader) and 
  [Render Script](https://github.com/DiligentGraphics/DiligentTools/tree/master/RenderScript), a Lua-based run-time 
  graphics resource managing system. Tools module depends on Core module.
* [Samples](https://github.com/DiligentGraphics/DiligentSamples) submodule contains several simple graphics applications 
  intended to demonstrate the usage of the Diligent Engine API. The module depends on Core and Tools modules.
  

# Build Instructions

Diligent Engine uses [CMake](https://cmake.org/) as a cross-platform build tool. 
To start using cmake, download the [latest release](https://cmake.org/download/) (3.10 or later is required for Windows build).

## Win32

To generate build files for Windows desktop platform, use either CMake GUI or command line tool. For example, to generate 
[Visual Studio 2017](https://www.visualstudio.com/vs/community) 64-bit solution and project files in *cmk_build/Win64* folder, 
navigate to the engine's root folder and run the following command:

*cmake -H. -B./cmk_build/Win64 -G "Visual Studio 15 2017 Win64"*

**WARNING!** In current implementation, full path to cmake build folder **must not contain white spaces**. (If anybody knows a way
to add quotes to CMake's custom commands, please let me know!)

Open DiligentEngine.sln file in cmk_build folder, select the desired configuration and build the engine. By default, Asteroids
demo will be set up as startup project.


## Universal Windows Platform

To generate build files for Universal Windows platform, you need to define the following two cmake variables:

* CMAKE_SYSTEM_NAME=WindowsStore 
* CMAKE_SYSTEM_VERSION=< Windows SDK Version >

For example, to generate Visual Studio 2017 64-bit solution and project files in *cmk_build/UWP64* folder, run the following command
from the engine's root folder:

*cmake -D CMAKE_SYSTEM_NAME=WindowsStore -D CMAKE_SYSTEM_VERSION=10.0.15063.0 -H. -B./cmk_build/UWP64 -G "Visual Studio 15 2017 Win64"*

## Linux

Your Linux environment needs to be set up for c++ development. To configure my fresh Ubuntu 17.10, I installed the following packages:

1. gcc, make and other essential c/c++ tools:

* sudo apt-get update
* sudo apt-get upgrade
* sudo apt-get install build-essential

2. cmake

* sudo apt-get install cmake

3. Other required packages:

* sudo apt-get install libx11-dev
* sudo apt-get install mesa-common-dev
* sudo apt-get install mesa-utils
* sudo apt-get install libgl-dev

To generate make files for debug configuration, run the following CMake command from the engine's root folder:

*cmake -H. -B./cmk_build/Linux64 -G "Unix Makefiles" -DCMAKE_BUILD_TYPE="Debug"* 

To build the engine, run the following command:

*cmake --build ./cmk_build/Linux64*


## Android

Please make sure that your machine is set up for Android development. Download 
[Android Studio](https://developer.android.com/studio/index.html), [Android NDK](https://developer.android.com/ndk/downloads/index.html) and
other required tools. To verify that your environment is properly set up, try building 
[teapots sample](https://github.com/googlesamples/android-ndk/tree/master/teapots).

Open *DiligentSamples/Android* or *UnityPlugin/Android* folders with Android Studio to build and run
the engine samples and Unity emulator on Android.

# Tutorials

## [Tutorial 01 - Hello Triangle](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial01_HelloTriangle)

![](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial01_HelloTriangle/Screenshot.png)

This tutorial shows how to render a simple triangle using Diligent Engine API.

## [Tutorial 02 - Cube](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial02_Cube)

![](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial02_Cube/Screenshot.png)

This tutorial demonstrates how to render an actual 3D object, a cube. It shows how to load shaders from files, create and use vertex, 
index and uniform buffers.

## [Tutorial 03 - Texturing](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial03_Texturing)

![](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial03_Texturing/Screenshot.png)

This tutorial demonstrates how to apply a texture to a 3D object. It shows how to load a texture from file, create shader resource
binding object and how to sample a texture in the shader.

## [Tutorial 04 - Instancing](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial04_Instancing)

![](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial04_Instancing/Screenshot.png)

This tutorial demonstrates how to use instancing to render multiple copies of one object
using unique transformation matrix for every copy.

## [Tutorial 05 - Texture Array](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial05_TextureArray)

![](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial05_TextureArray/Screenshot.png)

This tutorial demonstrates how to combine instancing with texture arrays to 
use unique texture for every instance.

## [Tutorial 06 - Multithreading](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial06_Multithreading)

![](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial06_Multithreading/Screenshot.png)

This tutorial shows how to generate command lists in parallel from multiple threads.

## [Tutorial 07 - Geometry Shader](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial07_GeometryShader)

![](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial07_GeometryShader/Screenshot.png)

This tutorial shows how to use geometry shader to render smooth wireframe.


## [Tutorial 08 - Tessellation](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial08_Tessellation)

![](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Tutorials/Tutorial08_Tessellation/Screenshot.png)

This tutorial shows how to use hardware tessellation to implement simple adaptive terrain 
rendering algorithm.


# Samples

[Sample source code](https://github.com/DiligentGraphics/DiligentSamples)

## AntTweakBar Sample

This sample demonstrates how to use [AntTweakBar library](http://anttweakbar.sourceforge.net/doc) to create simple user interface. 
It can also be thought of as Diligent Engine’s “Hello World” example. 

![](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Samples/AntTweakBar/Screenshot.png)

## Atmosphere Sample

The sample demonstrates how Diligent Engine can be used to implement various rendering tasks: 
loading textures from files, using complex shaders, rendering to textures, using compute shaders 
and unordered access views, etc.

![](https://github.com/DiligentGraphics/DiligentSamples/blob/master/Samples/Atmosphere/Screenshot.png)

# Projects

## Asteroids Performance Benchmark

This sample is designed to be a performance benchmark and is based on 
[this demo](https://software.intel.com/en-us/articles/asteroids-and-directx-12-performance-and-power-savings) developed by Intel. 
It renders 50,000 unique textured asteroids. Every asteroid is a combination of one of 1000 unique 
meshes and one of 10 unique textures. The sample uses original D3D11 and D3D12 native implementations, 
and adds implementation using Diligent Engine API to allow comparing performance of different rendering modes.

![](Projects/Asteroids/Screenshot.png)

## Unity Integration Demo

[This project](unityplugin) demonstrates integration of Diligent Engine with Unity

![](unityplugin/GhostCubePlugin/Screenshot.png)

# References

[Diligent Engine on the Web](http://diligentgraphics.com)

[Diligent Engine on Twitter](https://twitter.com/diligentengine)

[Diligent Engine on Facebook](https://www.facebook.com/DiligentGraphics/)

[API Reference][1]


# Version History

## v2.1.b

* Removed legacy Visual Studio solution and project files
* Added API reference
* Added tutorials 1-8

## v2.1.a

* Refactored build system to use CMake and Gradle for Android
* Added support for Linux platform

## v2.1

### New Features

#### Core

* Interoperability with native API
** Accessing internal objects and handles
** Createing diligent engine buffers/textures from native resources
** Attaching to existing D3D11/D3D12 device or GL context
** Resource state and command queue synchronization for D3D12
* Integraion with Unity
* Geometry shader support
* Tessellation support
* Performance optimizations

#### HLSL->GLSL converter
* Support for structured buffers
* HLSL->GLSL conversion is now a two-stage process:
** Creating conversion stream
** Creating GLSL source from the stream
* Geometry shader support
* Tessellation control and tessellation evaluation shader support
* Support for non-void shader functions
* Allowing structs as input parameters for shader functions


## v2.0 (alpha)

Alpha release of Diligent Engine 2.0. The engine has been updated to take advantages of Direct3D12:

* Pipeline State Object encompasses all coarse-grain state objects like Depth-Stencil State, Blend State, Rasterizer State, shader states etc.
* New shader resource binding model implemented to leverage Direct3D12

* OpenGL and Direct3D11 backends
* Alpha release is only available on Windows platform
* Direct3D11 backend is very thoroughly optimized and has very low overhead compared to native D3D11 implementation
* Direct3D12 implementation is preliminary and not yet optimized

### v1.0.0

Initial release

# License

Licensed under the [Apache License, Version 2.0](License.txt)

**Copyright 2015-2018 Egor Yusov**

[diligentgraphics.com](http://diligentgraphics.com)

[1]: https://cdn.rawgit.com/DiligentGraphics/DiligentCore/4949ec8a/doc/html/index.html
