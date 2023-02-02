# Session 3
#   Projet: Environmental conditions

This session should be done in 3 hours.
You are advised to refer the the C++ lectures and the links given below in order to
achieve the goals of this computing session.

## Goals
The goal of this computing session is to combine the codes developed during the computing sessions 1 and 2 in a new application.

## Skills to develop
  - Producing a shared library [UNIX or MacOS system]
  - Using a GNU Makefile [UNIX or MacOS system]
  - Using a class that you have not developed.

## Tools used
   - **Compiler**: 
       - the default compiler is **g++**.
   - **GNU Make**: relevant on UNIX/MacOS environment.
   - **Text editor**: feel free to use the editor of our choice:
       - several editors are available including emacs, gedit, nedit, vi/vim, ...
   - **Doxygen**: this tool is installed on the setup.

## Instructions

### Step 0: Preparing your work environment


  - Creating a folder devoted to Computing Session 3 code: 
	```
	   mkdir Session3
	   cd Session3
	```
	

In order to ease the realization of this computing session, it has been decomposed into short steps.

### Step 2: Using functions and classes from a library

The main goal is to create a new program with a main function aiming to read and analyze the data (temperature, pressure and humidity) supposely taken by the setup.
 - copy/paste the code developed during session 1 in order to read the temperature, pressure and humidity.
 - include in your project the three classes: **StatisticsCalculator**, **PsychrometricCalculator** and **PixelWriter**. Normally, you have developed one class during Session 2. The code of the two other classes must be borrowed from one of your colleagues (borrow the code but also the doxygen doc).
 - at this step, it is advised to instantiate the classes **StatisticsCalculator**, **PsychrometricCalculator** and **PixelWriter** in your ``main`` program which read SenseHat senesors and try to build (compile) the whole project.
 - with the class **PsychometricCalculator**, compute for each measure the dry temperature and the vapour pressure on top of all the other variables (energies etc ). 
 - with the class **StatisticsCalculator**, compute the relevant statistical quantities (min, max, mean, median, rms, std-dev) and dump them first at the screen.
 - save all corrected data (temperature, pressure, humidity, dry temperature and vapour pressure) then into a CSV file. The first value of each line will be the name of the variable, **i.e.** "energy1" etc.
 - withe the class **PixelWriter**, display on the 8x8 LED matrix the mean temperature and the mean relative humidity.
 

### Step 3: Using GNU Makefile

 **Relevant for UNIX and MacOS users.**

 - Relevant instructions to understand the goal and the usage of Makefile can be retrieve in an earlier ESIPAP session [here](https://indico.cern.ch/event/782305/contributions/3256094/attachments/1795957/2928175/Makefile.pdf).
 - After reading this document, write down a Makefile and compile our project with it.


### Step 4: Production a shared library
  For UNIX and MacOS users, you can follow the instruction given in the lecture in order to create a shared library. Assuming that you can a class written in a file written.cpp, you can use the command line below.
   - ``` g++ -c class.cpp``` 
   - ``` g++ -fPIC -shared -o libESIPAP.so class.o```

You need to modify system variable in order to allow the system to search for the library in the directory where it is located (command for bash shell and assuming that you execute it in the directory in which the library can be found):
``` export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:`pwd` ```

While compiling the main program, you need to link the library:

```g++ -L`pwd` -lESIPAP main.cc -o main```

  assuming that the program is called main.cc and that the library is located on the current directory.

The previous command lines may be changed in you use a different shell or if the library is located on a different directory.

The command lines can be enriched if you have several files to be merged in the library (several classes or functions)

If you are successful, you can enrich our Makefile to produce the library and to compile our main program.

### Step 5: Improving the configuration

It is possible to pass arguments to the programm in the command as the main function can be prototyped as

```int main(int argc, char* argv[])```

After looking to the lecture or any relevant in reference, modify our program to retrieve the name of the file to be read in the command line such as following:

```./main file.csv```

Protect our code by checking that an argument was provided. If it is not the case, print a message to inform the user on the required argument.

### Step 6: Going further

You can enrich you program in order to propose several options:
 - the code can check the extension of the file given as argument to know if the format is  binary  (.dat) or ascii (.csv). Adapt the code for the 2 cases **PsychrometricCalculator** and **StatisticsCalculator**.
- the user could specify the values of interest (s)he may to display using predefined key word such as "mean", "median",... for the class StatisticCalculator or "enthalpy" etc for the class PsychrometricCalculator
 - te user could also specify the name of the output file with the option -o
