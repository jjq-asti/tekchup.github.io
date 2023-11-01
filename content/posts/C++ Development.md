+++
title = "C/C++ Development in Emacs"
author = ["jeanjayquitayen"]
date = 2023-10-30T00:00:00+08:00
categories = ["Tutorials"]
draft = false
+++

## Introduction {#introduction}

I do most of my software projects in Doom Emacs.
I use python most of the time until recently when I decided to learn C and C++ again for a work related project.
I do have some experience in C since I do embedded projects back at school and in some work related projects before.
But there are still many concepts that I haven't learned yet or already forgotten, most specially in C++.

The features that I am looking for are code completion, syntax checking and linting.
To get this features I need to active `lsp` and install the LSP server on the host machine.
The C/C++ language mode should also be activated in Doom Emacs.


## Install the LSP server {#install-the-lsp-server}

I use [ccls](https://github.com/MaskRay/ccls) for the language server because it is the most suggested server and it works great in my experience.

I installed it using my system's package manager.
`sudo zypper install ccls`
This should also be available on other distros (i.e. Ubuntu).


## Setup emacs {#setup-emacs}

Activate lsp and C/C++ mode in the `init.el` file at the `:tools` section.

```emacs-lisp
(lsp)
```

and activate C/C++ languange in `:languages` section

```emacs-lisp
(cc +lsp)
```

Reload doom emacs by pressing `SPC-h r r`.
I am running emacs in client mode so I also restart emacs to be sure.

```shell
systemctl --user restart emacs
```


## Testing {#testing}

To test the setup, I created a simple project in C++ and cmake.
The project will have a function to add two numbers which will be compiled as library.
This will then linked to the final executable.

Create the project directories

```shell
mkdir proj
```

Then create the directories inside it

```text
proj/src
proj/lib
proj/include
```

Create the function prototype.

```c++
int add_numbers(int num1, int num2);
```
<div class="src-block-caption">
  <span class="src-block-number">Code Snippet 1:</span>
  include/mymath.hpp
</div>

Implement the function for our library.

```c++
#include "mymath"

int add_numbers(int num1, int num2){
    return num1 + num2;
}
```
<div class="src-block-caption">
  <span class="src-block-number">Code Snippet 2:</span>
  lib/mymath.cpp
</div>

Call the function in the **main.cpp**

```c++
#include <iostream>
#include "mymath"

int main(void){
   std::cout << add_numbers(2, 4) << std::endl;

   return 0;
}
```
<div class="src-block-caption">
  <span class="src-block-number">Code Snippet 3:</span>
  src/main.cpp
</div>

Write the cmake file.

```cmake
cmake_minimum_required(VERSION 3.27)

add_subdirectory(${PROJECT_SOURCE_DIR}/lib)

add_executable(app ${PROJECT_SOURCE_DIR}/main.cpp)
target_link_libraries(app mymath)
```
<div class="src-block-caption">
  <span class="src-block-number">Code Snippet 4:</span>
  CMakeLists.txt
</div>

Create another **CMakeLists.txt** in the lib directory.

```cmake
cmake_minimum_required(VERSION 2.7)

add_library(mymath mymath.cpp)
target_link_directories(mymath PRIVATE ${PROJECT_SOURCE_DIR}/include)
```
<div class="src-block-caption">
  <span class="src-block-number">Code Snippet 5:</span>
  lib/CMakeLists.txt
</div>

Create a **build.sh** file.

```shell
cmake -S . -B build
cmake --build build
```
<div class="src-block-caption">
  <span class="src-block-number">Code Snippet 6:</span>
  build.sh
</div>

Make it executable with `chmod +x build.sh`

We can then compile the project.
Press `SPC c c` to run emacs compile command.
It will then ask for the actual command for the compilation.
We can run the command `./build.sh` or `sh build.sh`.
If you are editing the main.cpp the command won't be able to find the **build.sh**.
That is because the compile command is run in the directory that you are working with.
In the case of main.cpp that is inside the **src** directory.
To temporarily go back to the root of the project we can use the bash env command and tell it to change directory before running the command.

```shell
env --chdir=.. ./build.sh
```
