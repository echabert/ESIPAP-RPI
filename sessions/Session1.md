# Computing session 1: Acquiring data from sensors with a C++ program

This session should be done in 3 hours.
You are advised to refer the the C++ lectures and the links given below in order to
achieve the goals of this computing session.

## Goals
The subject of this first computing session is voluntarily simple in order to allow you to acquaint yourself with the environment and good practices of C++ development. During this session, you must write in C++ a program from scratch and step-by-step. 

 During this session, We will use the setup as a tool to monitor the environment conditions are it is often necessary in test bench or beam test while operating detector (such as silicon sensors by example).
The program to design and to code must be able to read data, acquired by a sensor device and encoded in a binary file, to decode them and to save them into a CSV which can be read by a spreadsheet like Excel or OpenCalc.

## Skills to develop
  - Becoming familiar with C++ development environment (compiler, linker, text editor, ...).
  - Writing a simple C++ program.
  - Using the STL classes for reading/writing a file and for displaying informations at the screen.
  - Structuring the program in functions and in different source files.
  - Using classes developed by a third party.
  - Compiling and linking a project made up of several source files
  - Saving and sharing the C++ code with a version-control system (Git in our cases).
  - Improving the robustness of the code in order to prevent abnormal temrination or unexpected actions.

## Tools used
   - **Compiler**: 
       - on Linux/MacOSX machines, the default compiler is **g++**
   - **Text editor**: feel free to use the editor of our choice:
       - on the Linux virtual machine, several editors are available including emacs, gedit, nedit, vi/vim, ...
   - **Git**: requires to have an accound on [github](https://github.com/)

## Instructions

In order to ease the realization of this computing session, it has been decomposed into short steps.
At the end of each of them, you can compile and test our program before going further.

### Step 0: Preparing your work environment

#### Step 0.1: Downloading the instructions 

You must access to the last version of our instructions order to do the computing sessions. Please follow the instructions in order to have the updated code.

   - Open a Terminal 
   - Create a working folder:<br/>
        ```
        cd 
        mkdir esipap_instructions
	    cd esipap_instructions
		```
        ```
   - Download our main github repository by typing the command:
     ```
     git clone https://github.com/echabert/ESIPAP-RPI.git
     ```
     Comment: This last step can also be carried out by downloading a zip archive of the code (needed to be unzip)
   https://github.com/echabert/ESIPAP-RPI/archive/main.zip

#### Step 0.2: Creating your own repository

  - The goal of this section is to create a remote repository where you can save your results. You need to follow the instructions described in [the section Git Repository](../doc/git/GitRepository.md).

   - Open a Terminal:
        ```
        cd 
        mkdir esipap_sessions
	    cd esipap_sessions
        ```
   - Download your github repository by typing the command:
     ```
       git clone https://github.com/your_username/ESICppCS.git
     ```
     where your\_username as to be replaced by your username.
     A password will be asked.

#### Step 0.3: Creating your working folder for Computing Session 1

  - In a console session, entering the folder ```ESICppCS``` by issuing the command lines:
	   ```cd ~/esipap_sessions/ESICppCS```
  
  - Creating a folder devoted to Computing Session 1 code: 
	```
	   mkdir Session1
	   cd Session1
	```
	
  - In the folder ```Session1```, creating a new text file called ```readme.txt``` containing the sentence ```Folder corresponding to ESIPAP-2021 Computing Session 1```
	
  - Telling Git that you have added a new file by issue the command line in the console:
      ```git add readme.txt```
	  
  - Recording the changes to the local repository with the following command:
	  ```git commit -m 'add readme.txt file` readme.txt```
	  
  - Propagating the changes to the remote repository with the following command:
	  ```git push```


### Step 1: Creating, building and running a 'Hello World!' program

   - In the folder `Session1` folder, creating a source file called `helloworld.cpp` with the following content:
     ```
       #include<iostream>
       using namespace std;

       int main()
       {
	     // Core program
	     cout << "Hello World!" << endl;
	
	     // Wait that user pushes a button (relevant for Windows system)
	     cout << "Push a button to quit" << endl;
	     cin.get();
		 
		 // Return no error code
	     return 0;
       }	   
	 ```
	 
   - In order to build this program (compilation + link + creation of an executable file called `helloworld`) you need to type the command `
	     ```
		   g++ -o helloword helloworld.cpp
		 ```
   - Executing the program:
       - On Linux/MacOSX machines: ```./helloworld```
	   - On Windows machines by using command lines: ```helloworld```
	   - On Windows machines by using the Visual Studio interface: click on the button "Local Windows Debugger"
	  
   - Saving the code in the remote repository:
	
	  - Telling Git that you have added a new file by issue the command line in the console:
      ```git add helloworld.cpp```
	  
      - Recording the changes to the local repository with the following command:
	  ```git commit -m 'add helloworld program' helloworld.cpp```
	  
      - Propagating the changes to the remote repository with the following command:
	  ```git push```
	  

### Step 2: Handling the SenseHat board with C++ library and writing a acquisition program

 Technically, a C++ library called [RTIMULib](https://github.com/RPi-Distro/RTIMULib) has been developed and is installed on the setup.
 It contains several classes useful to communicate with the SenseHat elements. In order to access those functionalities, it will be required to link this library during the compilation.
 In the goal of simplifying the access to the SenseHat board, we have developed a new class called **SenseHat** which uses several classes contained in libRTIMULib.so.

#### Step 2.1: Inclusion of the class SenseHat

 Open the header of the SenseHat class in order to understand its structure. 
 Both the structure and the language keywords should have been presented during the leture.
 If you have questions, you can directly contact the teachers.

  - Extend the "Hello world" example by instantiating an object of the class **SenseHat** and by initializing it.
  - Compile the program linking the executable with the library **libSenseHat.so**.
  - "Automatize" the compilation by writing down the compilation commands in a bash script **mymake**. We will later learn how to create a **Makefile**

#### Step 2.2: First acquisition program
  By reading the header of the SenseHat class, you need to learn the available methods that will be useful to realize the tasks required below.

  - Read 10 times both the temperatures measured by the pressure and the humidity sensors every second and display them on the terminal (using std::cout)/
  - Make our program more configurable by asking the user the number of measurements and the delay.
  
  Remarks:
  - The delay can be generated by using a method of the class SenseHat. It uses the UNIX command sleep which use delay expressed in microseconds.
  - Use std::cin to retrieve parameters entered by the user.
  - It is also possible to retrieve values through the arguments given in the command line (arguments of the function main).


### Step 3: Exporting measurements

It is often useful to disentangle the acquisition program from the data analysis one. The goal of the acquisition program FirstDAQExport.cc to be written is to store data in a given numerical format. 
For the sake of simplicity, we will store the data in a CSV (Comma Separated Value) format. 
This imply that variables of a given measurement (once per line) are separated by a comma.

  - Write a first version of the program that store in a CSV file the following values:
     - the time in microseconds since the first measurement
     - the pressure [bar]
     - the relativity humidity
     - the temperature measured by the pressure sensor [◦C]
     – the temperature measured by the humidity sensor [◦C]
     – both temperatures from the CPU and GPU of the Raspberry Pi [◦C]
  - Protect the code against unavailable measurement by adding a default value (-9999), allowing a offline treatment
  - Modify the program to write this functionnality as a function which takes as arguments the number of measurements and the delay.
  - Split the project in 3 files: a file CSVExport.h and CSVExport.cc which contain respectively the prototype and the implement of the function as well as a main program which call the function.
  - Write the compilation instructions in a script **mymake** and compile the project.
  - Test if the program is working properly. The integrity of the CSV file can even be tested by loading it with **OpenOffice Cal**. Change the delimeter for CSV file in the configuration to use "," and not ";".

 ### Step 4: Measurement campaigns

 Measurements in various conditions can be done, allowing dedicated analyzes in the next computing sessions. 
 In order to facilitate the execution of several runs with varying conditions,modify the main program to read from the command line the delay, the number of measurements and the output csv filename.

 A list of measurements is proposed on the table below. It would be interested for analysis purpose. Take de delay of 1 sec by default.

 |Name | Duration [min] | Comments|
 |Stable conditions | 5 | |
 |High rate   | 0.25 | scan short delay [ms or micros] | 
 |Humidity    | 3    | blow on the SenseHat board after 30'|
 |Temperature | 3    | run an air dryer after 30' |
 |Activity - 1| 4    | run *hot.py* |
 |Activity - 2| 4    | launch *video.avi* |
 |Activity - 3| 4    | run *hot.py* - SenseHat on top of RPI |
 |Activity - 4| 4    | launch *video.avi* - SenseHat on top of RPI |
 |Outside     | 6    | move the setup from inside to outside |


### Step 4 (optional): Display with the LED matrix

The Sense Hat board is equiped with a LED matrix. It is possibe to change the color of each
of the individual pixels via methods of the class SenseHat. 
Functionnalities are available on the SenseHat class but remain basics.
In the next session, it will be propose to develop a class to write sentences and numbers with the LED matrix.
Meanwhile, in this section, you are free to use the LED matrix to display visual information playing directly with the individual pixels.
Here is a list of possible small projects:
 - represent the value of the measured observables (temperature, humidity) on a 8 LED line (you could use 2 different lines)
 - use the range 0-64 pixels to reprensent the intensitiy of the temperature or humidity
 - use the y-axis to represent the intensity while the x-axis is used to represent the time: evolution of the temperature or humidity vs time
 - make the LED blinking if the measurement is above it a predefined threshold
 - ...



