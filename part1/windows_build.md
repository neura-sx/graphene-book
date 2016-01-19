# Windows 64-bit - Visual Studio 2015
### Prerequisites
* This build is for `Windows 7 64-bit` or `Windows 10 64-bit`.
* We assume the following applications are already installed:
  * [7-zip](http://www.7-zip.org)
  * [Github desktop](https://desktop.github.com)
  * [Visual Studio 2015](https://www.visualstudio.com) (the Community version is free)

> When installing VS please choose custom installation and make sure the `Common Tools for Visual C++ 2015` feature is selected. In the features tree it is located here:  
`Programming Languages -> Visual C++ -> Common Tools for Visual C++ 2015`.

### Folder structure
* Create a new folder, e.g. `C:\Users\[your user name]\Documents\Graphene-Main`.  
This is where BitShares source code, binaries and Visual Studio solution will be stored.  
This folder needs at least 5.5 GB of free space.

* Create a new folder, e.g. `C:\Users\[your user name]\Documents\Graphene-Aux`.  
This is where auxiliary libraries and tools will be stored.  
This folder needs at least 3.5 GB of free space.

Those folders can be located wherever you like and can be named whatever you prefer.  
For convenience, from now on those folders will be referred to as `[Graphene-Main]` and `[Graphene-Aux]`.  
If you want, you can keep everything in one folder so that `[Graphene-Aux]` is the same as `[Graphene-Main]`.

### Downloads

#### Fetch CMake
We will use a zipped version available here:  
[https://cmake.org/files/v3.4/cmake-3.4.1-win32-x86.zip]()  
Unzip the file and move it to `[Graphene-Aux]\cmake-3.4.1-win32-x86`.

> Make sure you don't end up with nested folders, i.e. you should have:  
`[Graphene-Aux]\cmake-3.4.1-win32-x86\bin`  
instead of something like this:  
`[Graphene-Aux]\cmake-3.4.1-win32-x86\cmake-3.4.1-win32-x86\bin`.

#### Fetch Boost  
We will use a precompiled version available here:  
[http://netcologne.dl.sourceforge.net/project/boost/boost-binaries/1.60.0/boost_1_60_0-msvc-14.0-64.exe]()  
Run the downloaded exe file and when prompted choose the deployment location as `[Graphene-Aux]\boost_1_60_0`.  

Once Boost is successfully deployed, you should end up with `[Graphene-Aux]\boost_1_60_0\lib64-msvc-14.0` containing compiled 64-bit Boost libraries.  

> If you don't trust this exe file, get the Boost compilation from elsewhere (or compile it from source) and make sure that the 64-bit compiled libraries are located in `[Graphene-Aux]\boost_1_60_0\lib64-msvc-14.0`.

#### Fetch OpenSSL  
We will use a precompiled version available here:  
[www.npcglib.org/~stathis/downloads/openssl-1.0.1q-vs2015.7z]()  
Extract the content of this archive using 7-zip to `[Graphene-Aux]\openssl-1.0.1q-vs2015`.  

> Make sure you don't end up with nested folders, i.e. you should have:  
`[Graphene-Aux]\openssl-1.0.1q-vs2015\bin`  
instead of something like this:  
`[Graphene-Aux]\openssl-1.0.1q-vs2015\openssl-1.0.1q-vs2015\bin`.  

Inside the `[Graphene-Aux]\openssl-1.0.1q-vs2015` folder we need to do some renaming:  
* rename `bin` to `bin32` and then `bin64` to `bin`  
* rename `include` to `include32` and then `include64` to `include`  
* rename `lib` to `lib32` and then `lib64` to `lib`

> If you don't trust this precompiled version, get the OpenSSL compilation from elsewhere (or compile it from source) and make sure that the 64-bit binaries, includes and libraries are located in `bin`,`lib` and `include` folders. The important thing is that OpenSSL needs to be compiled in MSVC 2015 because previous compilations are incompatible with VS 2015 as described [here](http://stackoverflow.com/questions/30412951/unresolved-external-symbol-imp-fprintf-and-imp-iob-func-sdl2).

#### Expected result
As a result, you should end up with these three folders inside `[Graphene-Aux]`:
```
boost_1_60_0
cmake-3.4.1-win32-x86
openssl-1.0.1q-vs2015
```

### System path variable
Add `[Graphene-Aux]\openssl-1.0.1q-vs2015\bin` to your system path variable.  
To do this, open the Control Panel and navigate to `System and Security -> System\Advanced System Settings -> Environment Variables` and edit the system path variable named `Path`.

### BitShares source code
Open your power shell console. Normally it is located here:  
`C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`  
but this location can differ depending on your OS settings.  

In the power shell console run the following commands:  
```
cd [Graphene-Main]
git clone https://github.com/bitshares/bitshares-2.git
cd bitshares-2
git submodule update --init --recursive
```
As a result, you should have the folder `[Graphene-Main]\bitshares-2` populated with BitShares2 source code.  
You can close the powershell console now.

### CMake amendmends
There are two little hacks needed to make CMake pre-processing work on Windows.  
* Edit file:  
`[Graphene-Main]\bitshares-2\CMakeLists.txt`  
Line 35:  
`list( APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/libraries/fc/CMakeModules" )`  
needs to be commented out, i.e. changed to this:  
`# list( APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/libraries/fc/CMakeModules" )`

* Edit file:  
`[Graphene-Main]\bitshares-2\libraries\fc\GitVersionGen\GetGitRevisionDescription.cmake`  
Line 133:  
`set(${_var} "GIT-NOTFOUND" PARENT_SCOPE)` (i.e. within the `get_git_unix_timestamp` function)  
needs to be changed to something like this:  
`set(${_var} "888" PARENT_SCOPE)`  
> The value `888` is just a random number, it could  be anything e.g. `123` - it just needs to be a number (enclosed by double inverted commas), not text.  
Make sure you don't confuse line 133 with line 89 which looks the same but it's within the `git_describe` function.

### Run CMake
The purpose of CMake is to create a Visual Studio solution for the BitShares source code.

The `[Visual-Studio-Home]` placeholder stands for Visual Studio 2015 home folder, e.g.  
`C:\Program Files (x86)\Microsoft Visual Studio 14.0`  

Open a standard command prompt console and run the following commands:
```
cd [Graphene-Main]
set GRAPHENE_AUX=[Graphene-Aux]
set BOOST_ROOT=%GRAPHENE_AUX%\boost_1_60_0
set BOOST_LIBRARYDIR=%BOOST_ROOT%\lib64-msvc-14.0
set OPENSSL_ROOT_DIR=%GRAPHENE_AUX%\openssl-1.0.1q-vs2015
"[Visual-Studio-Home]\VC\vcvarsall" x86_amd64
"[Graphene-Aux]\cmake-3.4.1-win32-x86\bin\cmake-gui" -G "Visual Studio 14 Win64"
```
> Make sure you replace all the placeholders (e.g. `[Graphene-Main]`) with appropriate paths.

As a result the CMake GUI should appear on your screen.

In the GUI, enter the following path in the source code field: `[Graphene-Main]\bitshares-2` and the following path in the binaries field: `[Graphene-Main]\bin64`. And then hit `Configure`.  

When prompted, confirm the creation of a new folder called `bin64`.  

A new pop-up window should appear. Please select `Visual Studio 14 2015 Win64` and `Use default native compilers` and then hit `Finish`. 
There will be some red warnings displayed in the output window of the GUI but these can be safely ignored.  

If successful, the process should end with the message `Configuring done` displayed in the GUI.  
Now hit `Generate`. And once you get the message `Generating done` you can exit the CMake GUI and also close the command prompt console.  

As a result, there should be a Visual Studio solution structure located in `[Graphene-Main]\bin64`, including the `BitShares.sln` file.

### Run Visual Studio
We are now ready to compile the source code in Visual Studio.  
Start Visual Studio 2015 and open the BitShares solution (generated by CMake) which should be located here:  
`[Graphene-Main]\bin64\BitShares.sln`  

In the top tool bar set active configuration to `RelWithDebInfo` and make sure the platform choice is set to `x64`.

We need to apply two little hacks before we can try to compile:
* In the preprocessor settings for the `fc` project add a new definition for a variable called `_WEBSOCKETPP_NOEXCEPT_`.  
To do that, right-click on the `fc` project, open the properties panel, navigate to `Configuration Properties -> C/C++ -> Preprocessor -> Preprocessor Definitions` and add `_WEBSOCKETPP_NOEXCEPT_` to the list.

* In the `graphene_chain` project open a source file called `fee_schedule.cpp` and below line 22  
`#include <fc/smart_ref_impl.hpp>`  
add a new line with this content:  
`#include <algorithm>`.

Now we are ready to compile. Choose `Build Solution` from the main menu to start building the entire solution. It will take a while but the task should complete without any errors and with all 47 projects successfully built.
