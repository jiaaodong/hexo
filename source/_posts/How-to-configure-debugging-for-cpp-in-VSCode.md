---
layout: default
title: How-to-configure-debugging-for-cpp-in-VSCode
date: 2019-01-13 18:31:09
tags: Tools, Ubuntu
---

# Abstraction:

This article describes how to configure the C++ program debugger in Visual Studio Code(VSC from now on) on Ubuntu 18.04. As supplementary knowledge, the compilation process of C++ program is explained in the second part.

The tutorials on websites are either too old to use(since VSC is a fast developing software toolkit) or too simple without details and explanations.

# Part 1: Configuration the debugger in VSC

## 1. Install the extension 
VSC now has an excellent extension for C++ with a lot of user-friendly features. The first step to debug C++ code is to install this extension. Search the extension in VSC and install it.
![extension](http://pjy6fsofa.bkt.clouddn.com/cpp_extension.png)

## 2. Install GCC
GCC is one of the most commonly used compilers in Ubuntu system or other Linux systems. Install GCC if you don't have it on your computer by:
```bash
sudo apt-get update
sudo apt-get install gcc
```

## 3. Create a cpp file
```bash
mkdir -p ~/HelloWorld/src
cd /HelloWorld/src
```
Create a file called __helloworld.cpp__ in VSCode. And copy the code below in the file:
```cpp
#include <iostream>

int main(int argc, const char * argv[]) {
    std::cout << "hello Visual Studio Code! :)" << '\n'; 
    return 0;
}
```

_The arguments of the main function is mandatory, otherwise the debugging wouldn't go through itself freely. It will be stuck after the debug button is pressed._
## 4. Create a CMakeLists.txt
This can be done by cmaketools extension in VSC. But creating automatically means lack of flexibility. So I still choose to create the CMakeLists.txt file manually.

Create the __CMakeLists.txt__ file under the directory __HelloWorld__ with following content:
```Cmake
cmake_minimum_required(VERSION 3.0)

project(hello-vsc)

set(SOURCE helloworld.cpp)

add_executable(${PROJECT_NAME} ${SOURCE})
```

This is the simplest version which specifies the cmake requirement, project name and adds an executable. `set` means setting the variable `SOURCE` as `helloworld.cpp`. Then the variable name `SOURCE` can be used in other parts within this file.

## 5. Configure the external tasks
C++ source code needs to be built to generate the executables. The compilation process is in Part 2. In this step, a __tasks.json__ file is created to perform command line tasks automatically. It is like definining a macro in VSC. 

>Lots of tools exist to automate tasks like linting, building, packaging, testing or deploying software systems. Examples include the TypeScript Compiler, linters like ESLint and TSLint as well as build systems like Make, Ant, Gulp, Jake, Rake and MSBuild.
These tools are mostly run from the command line and automate jobs inside and outside the inner software development loop (edit, compile, test, and debug). Given their importance in the development life-cycle, it is very helpful to be able to run tools and analyze their results from within VS Code. [This is from the official website](https://code.visualstudio.com/docs/editor/tasks#vscode)

### 5.1 Create the tasks.json file in VSC
Press "Ctrl + Shift + P" to open the so called command palette in VSC. Then type `Tasks:Configure Task` (Actually this will be auto-completed by VSC). Choose `Create tasks.json file from template` and then choose `Others`. A new tasks.json template will pop out. 

Step 1:
![task1](http://pjy6fsofa.bkt.clouddn.com/Task1.png)

Step 2:
![task2](http://pjy6fsofa.bkt.clouddn.com/task2.png)

Step 3:
![task3](http://pjy6fsofa.bkt.clouddn.com/task3.png)

_VSC uses many configuration json files including tasks.json and the one which will be mentioned in the next step called launch.json. These files are saved in folder __.vscode__._

### 5.2 Edit the properties of tasks.json file

There are many properties in json files of VSC such as [the tasks.json properties](https://code.visualstudio.com/docs/editor/tasks#vscode).

In our case, we use a tasks.json file as follows:
```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "cmake",
            "type": "shell",
            "command": "cmake -G 'Unix Makefiles' -DCMAKE_BUILD_TYPE=Debug ..",
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": false,
                "panel": "shared",
                "showReuseMessage": true,
                "clear": false
            },
            "options": {
                "cwd": "${workspaceRoot}/build"
            }
            
            
        },

        {
            "label": "make",
            "type": "shell",
            "command":"make -j 8",
            "options": {
                "cwd": "${workspaceRoot}/build"
            }

        },

        {
            "label": "build",
            "dependsOn": [
                "cmake",
                "make"
            ]
        }
    ]
}
```

Here are the meanings of the properties:

`label`: The text content that will be displayed when you select the tasks in your tasks list when you run the tasks in command palette by first pressing "shift + ctrl + p" and then type `Tasks: Run Task`.

`type`: Whether the tasks is a command line task(in shell) or a process. 

`command`: The shell command that we need to type in the command line like we usually do in Terminal.


`presentation`: Some settings for output. This is not mandatory.

`options`: This is important. The `options` property is used for 
>Override the defaults for cwd (current working directory), env (environment variables), or shell (default shell). Options can be set per task but also globally or per platform. 

As we know we need to type `cmake` and `make` in __build__ directory. So the current working directory should be modified to build directory under our project folder, which means we need to create the directory in advance.

```shell
mkdir build
```

`cwd`: Current working directory

`depends`: This is the dependancy between tasks. We need this hierarchical structure of tasks because when we refer to the tasks in launch.json file in the next step, only one task can be specified as the tasks before running launch file.

There is another variable that needs to be explained, the `${workspaceRoot}`. This is a variable reference for the project folder. More information of variable reference in [here](https://code.visualstudio.com/docs/editor/variables-reference)



` "cmake -G 'Unix Makefiles` is the command for cmake. The flag `-G` is specified because we want the debug executable without optimization, in comparison with the Release. More flags in cmake command [here](https://cmake.org/cmake/help/v3.13/manual/cmake.1.html).


_The example in [VSCode official tutorial for c++](https://code.visualstudio.com/docs/languages/cpp) doesn't use other tools such as CMake and Make. But we really need a cross-platform build system. The reason is in the 2nd part of this article._


# 6. Configure the launch file
## 6.1 Create launch.json file
Go to the tab for debug. There would be a small gear for configuration of the launch.json file. Press the gear and commands in command palette will appear. Choose C++(GDB/LLDB) and a launch.json file will pop out.

Step 1:

![launch1](http://pjy6fsofa.bkt.clouddn.com/configure_launch_gear.png)

Step 2:

![launch2](http://pjy6fsofa.bkt.clouddn.com/GDB.png)

## 6.2 Edit launch.json file

The launch.json file is a file to configure your debugger. In python language this is automatically generated while in C++ you need to configure it by yourself.

The content of the launch.json file could be:
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/build/hello-vsc",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "build"
            
        }
    ]
}
```

There are also some properties as we saw in __tasks.json__. [More details are here](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations). Now the most important ones are:

`program`: The file of the executable. The name `hello-vsc` is the project name we add in __CMakeLists.txt__. This is the executable name in this project.

`preLaunchTask`: The task that needs to be performed each time before launching this launch file.

_We use GDB or LLDB to debug source code. "GDB Project Debugger allows you to see what is going on `inside' another program while it executes -- or what another program was doing at the moment it crashed."_

## 7. Start debugging
After configuration of the launch.json file, now the source code is able to be debugged. In debug tab, press debug button or use shortcuts "F5". 

![debug](http://pjy6fsofa.bkt.clouddn.com/Debug.png)

# Part 2: From C++ text file to executables on Linux
Now as a supplementary knowledge, let's go through the building process of a C++ program from the text file to the executable.

## 1. Preprocessing, compiling and linking
There are three stages between c++ text file and the executables, preprocessing, compiling and linking. The three words combined is equal to the word "build".

__Preprocessing__ is simply copy and paste. The preprocessing syntax such as `#include` and `DEFINE` is telling the preprocessor to copy specific code from somewhere to the place you refer to. In this stage, it is the programming language (in our case, C++ code) that is manipulated.

__Compiling__ is the stage where the code is translated to machine language. The output of this stage is `.obj` file. Each text file will generate one object file. In this stage, some compilation error will emerge such as error for the variable names without being declared.

__Linking__ is the stage where different source files are linked together. Why do we need this stage? Because in some source files, there are only declarations for functions and libraries. Same code is used for all more than one source files. Then linking process tells the project where to find the definition of functions. The output of this stage is executables. In Windows operating system it is with the extension `.exe`. While in Linux it doesn't have extension. At this stage linking error will come such as the function without being defined. 


## 2. CMake, Make and Compiler
"Cmake" is a cross-platform build tool which generates make files, which are inputs for the build systems. "Make" is a build system. A build system is used for compiling and linking to generate an executable. GCC and g++ are compilers for C++ and C who are responsible for compiling code. 

Why do we need CMake, Make and Compilers at the same time?
At the begining, the programs are compiled manually and linked together. We can still compile a single file using only the compilers. In most IDE, this is still an option (e.g. in Visual Studio and Visual Studio Code). 

Compiling thousands of source code files and linked them one by one is not a good way for large project. Not mention there are different modes such as Release and Debug. Therefore, automatic build system is created such as "make".

>GNU Make is a tool which controls the generation of executables and other non-source files of a program from the program's source files. 

"Make" is a good system with automation. But different build systems are limited to different platforms. In windows and linux probably the build systems are different. Here comes CMake, a cross-platform build tool.

>CMake is an open-source, cross-platform family of tools designed to build, test and package software. CMake is used to control the software compilation process using simple platform and compiler independent configuration files, and generate native makefiles and workspaces that can be used in the compiler environment of your choice.

## Reference:
1. VSCode Official tutorials [C/C++ for Visual Studio Code (Preview)](https://code.visualstudio.com/docs/languages/cpp)
2. stackoverflow answer [How do I use CMake?](https://stackoverflow.com/questions/7859623/how-do-i-use-cmake/7859663#7859663)
3. stackoverflow answer [Understanding roles of CMake, make and GCC](https://stackoverflow.com/questions/39761924/understanding-roles-of-cmake-make-and-gcc)
4. YouTube video TheChernoProject [How the C++ compiler works](https://www.youtube.com/watch?v=LKLuvoY6U0I&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=4)
5. stackoverflow answer [How does CMake choose gcc and g++ for compiling?](https://stackoverflow.com/questions/12475056/how-does-cmake-choose-gcc-and-g-for-compiling)
6. [GDB homepage](https://www.gnu.org/software/gdb/)
7. Medium article [C++ Development using Visual Studio Code, CMake and LLDB](https://medium.com/audelabs/c-development-using-visual-studio-code-cmake-and-lldb-d0f13d38c563)
8. CMake Tool [Getting start](https://vector-of-bool.github.io/docs/vscode-cmake-tools/getting_started.html#gs-configuring)
9. stackoverflow anser [Visual Studio Code: running preLaunchTask with multiple tasks](https://stackoverflow.com/questions/51599106/visual-studio-code-running-prelaunchtask-with-multiple-tasks)
