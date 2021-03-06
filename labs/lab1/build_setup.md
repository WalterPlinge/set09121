---
title: "Build Setup"
keywords: build_setup
tags: [build_setup]
permalink:  build_setup.html
summary: build_setup
sidebar: home_sidebar
---

To begin here I am assuming you have a local repo, with the SFML submodule.

## Get Some Code
With a simple text editor, create a **main.cpp** file in the **practical_1** folder, input the following code:
```cpp
#include <SFML/Graphics.hpp>

int main(){
  sf::RenderWindow window(sf::VideoMode(200, 200), "SFML works!");
  sf::CircleShape shape(100.f);
  shape.setFillColor(sf::Color::Green);

  while (window.isOpen()){
      sf::Event event;
      while (window.pollEvent(event)){
      if (event.type == sf::Event::Closed){
        window.close();
      }
    }
    window.clear();
    window.draw(shape);
    window.display();
  }
  return 0;
}
```

This is the basic "Hello world" for SFML, we will use this to test everything is in-place an working.


## Building code with CMake
Now we need to create our development environment. 

If you were mad you could create makefile and do it like the 1990's hacker. 

If you were naive you may want to open up visual studio, create a new project, and spend 2 hours digging thourgh build settings. 2005 Called and it want is workflow back.

While C++ doesn't have a standardized package and build system (i.e, Pythons's pip, Nodes's npm), we have something that's pretty close: **CMake**

CMake allows you to write a **CMakelists.txt file**. In which you specify what your program is, where the source files are, and where any of it's needed dependencies are. From here Cmake will take that config fille and go and make you a perfect pre-setup visual studio solution. No need to touch configuration options in visual studio.

CMake has many more benefits,but what we care about is:
1. You only need to store CMakelists.txt files in your Repo, __**no huge VSsolutions.sln**__
1. CMakelists.txt play nice with git, you can easily see and track changes
1. CMake doesn't just build Visual Studio solutions, it can build Xcode, Clion, Eclipse, makefiles..etc, this is an important step into writing cross-platform code.

Cmake Downsides:
1. It's yet another new scripting langauge to learn


### Create the CMake script
With a simple text editor, create a **CMakeLists.txt** file in the **root** folder, input the following code:

```CMake
cmake_minimum_required(VERSION 3.11)
# Require modern C++
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(Games_Engineering)

#### Setup Directories ####
#Main output directory
SET(OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/")
# Ouput all DLLs from all libs into main build folder
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${OUTPUT_DIRECTORY})

#### Add External Dependencies ####
add_subdirectory("lib/sfml")
set(SFML_INCS "lib/sfml/include")
link_directories("${CMAKE_BINARY_DIR}/lib/sfml/lib")

#### Practical 1 ####
file(GLOB_RECURSE SOURCES practical_1/*.cpp practical_1/*.h)
add_executable(PRACTICAL_1 ${SOURCES})
target_include_directories(PRACTICAL_1 PRIVATE ${SFML_INCS})
target_link_libraries(PRACTICAL_1 sfml-graphics)
```

While that may look foreign, you can generally guess at what every line does. The good news is I'll provide all the CMake code you will need. 

### Creating the Solution, with CMake
If you are unfamiliar with CMake UI; Follow [This guide](https://github.com/edinburgh-napier/aux_guides/blob/master/cmake_guide.pdf)


**Remember to place the build folder *OUTSIDE* of the repo folder. Preferably your desktop, *NOT* your H drive**

{:class="important"}
**NEVER Build from your H drive!** 
Or a memorystick / External HDD
	

The build folder will **never** contain work you need to save or commit. All code resides in the source directory.

Once configured and generated, you can open the .sln file in the build folder. You should not need to touch any solution or project settings form within Visual Studio.
The solution is set up so you don't have to do much work yourself or even understand Visual Studio settings.

### Run The solution
Cmake should have generated a solution project for you in your build folder, open it.
Practical_1 should be available as a project within it. Compile and run it!

{:class="important"}
You should see a green circle.


## Saving your work
You should take this opportunity to commit and push your work. If you know the basics of git, this is nothing new.
```bash
git status
```
Running git status should show you all the files you have modified so far. We need to "Stage" or "add" these files. 

```bash
git add .
```
This is a shorthand to tell git that we want to commit everything.


```bash
git commit -m "SFML hello world working"
```
Now we run the actual commit, which will store the current version of all your ("Staged")files to the local repo. Note that this is only local, you now need to push it up to github.

```bash
git push
```

This is a light-speed gloss over what version control can do for you. If this is new and strange to you, you really should take some time to look through some online git tutorials and guides to get comfortable with what it does and how it works.

## Starting from scratch
If you want to work work on another pc, or at home. You obviously don't need to create a new repo.
The steps you need to do are simply
1. Clone/Pull the repo down from your github
1. Get/update SFML by running: ```git submodule update --init --recursive ```
1. Run Cmake to generate your build folder

The key here is that you only need to version control your **source** folder.
The build folder, generated by CMake is full of large junk that visual studio needs, you don't need to save this or even really care about what's in there. 
You can re-generate it anytime anywhere using CMake.

---
Previous step: [Repo setup](repo_setup)
Next step: [Pong](pong)
