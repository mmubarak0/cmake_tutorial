# CMakeList tutorial 
* ## first you need to create CMakeList.txt file in the root directory
    * ### on windows
        ```
        type nul >> "CMakeLists.txt"
        type nul >> "main.cpp"
        ```
        ` or `
        ```
        wsl touch CMakeLists.txt
        wsl touch main.cpp
        ```
    * ### on Linux 
        ```
        touch CMakeLists.txt 
        touch main.cpp
        ```

<!-- body -->
</br>
</br>
<h2 align="center"> inside CMakeLists.txt (basic structure)  </h2>  
</br>  
</br>  

    code CMakeLists.txt

or 

    vim CMakeLists.txt

then copy the code below 


~~~
cmake_minimum_required(VERSION 3.0)

# Disable in-source builds to prevent source tree corruption.
if(" ${CMAKE_SOURCE_DIR}" STREQUAL " ${CMAKE_BINARY_DIR}")
  message(FATAL_ERROR "
FATAL: In-source builds are not allowed.
       You should create a separate directory for build files.
")
endif()

# Project Name
project(HelloCMake)

# let the compiler use c++ 14
set(CMAKE_CXX_STANDARD 14)

# add flags here like -pthread 
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall ")

if(APPLE)
    message("This is an Apple System")
elseif(WIN32)
    message("This is a Windows System")
    # you may need to install minGW and then add the bin folder to your path

elseif(${CMAKE_SYSTEM_NAME} MATCHES Linux)
    message("This is a Linux system")
elseif(${CMAKE_SYSTEM_NAME} MATCHES Android)
    message("This is an Android System")
endif()

# exe file name , c++ source file name
add_executable(hello main.cpp)
~~~  

<h2 align="center"> inside main.cpp </h2>  
</br>  
</br>  

    code main.cpp

or 

    vim main.cpp

then copy the code below 

~~~
#include <iostream>
auto sum(int a, int b){
    return a + b;
}
int main() {
    std::cout<<"Hello CMake!"<<std::endl;
    std::cout<<"Sum of 3 + 4 :"<<sum(3, 4)<<std::endl;
  return 0;
}
~~~

</br>

* ## good practice to make a build folder
    * ### on windows
        > don't forget to add `path/to/MinGW/bin` folder to your path
        ```
        mkdir build
        cd build
        cmake -G "MinGW Makefiles" ..
        mingw32-make all
        ```

        you can make some alias and function to make this a little shorter
        ```
        function gcmake { cmake -G "MinGW Makefiles" .. }
        Set-Alias -Name make -Value mingw32-make.exe
        ```
        > if you add those two lines to powershell profile it will be saved to any new sessions you follow this [guide](https://www.howtogeek.com/50236/customizing-your-powershell-profile/)
        > first time you will see an error just open a new powershell window as an adminstrator and type the following command 
        ```
        Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
        ```
        then you can normally use ` gcmake ` instead of `cmake -G "MinGW Makefiles" ..`  
        and use `make` instead of `mingw32-make`


    * ### on Linux 
        ```
        mkdir build
        cd build
        cmake ..
        make all
        ```


</br>  
</br>
<!-- body -->


* ## Variable values can be accessed with
    ```
    ${variable_name}
    ```
    ### example ...
    ```  
    set(TRIAL_VARIABLE "VALUE")
    message("${TRIAL_VARIABLE}")

    set(files a.txt b.txt c.txt)
    # sets files to "a.txt;b.txt;c.txt"
    # to access them we use loop
    foreach(file ${files})
        message("Filename: ${file}")
    endforeach()
    ```
* ##  Some commonly used commands:

    * prints given message 
        ``` 
        message
        ``` 
    * sets minimum version of cmake to be used
        ```
        cmake_minimum_required 
        ```
    * adds executable target with given name
        ``` 
        add_executable 
        ```
    * adds a library target to be build from listed source files
        ``` 
        add_library 
        ```
    * adds a subdirectory to build
        ```
        add_subdirectory  
        ```

## CMake Best Practices
* Always remember the previous configurations, make sure you append new flag instead of overwriting it
    ```
    set(VARIABLE "${VARIABLE} Flag1 Flag2")
    ```

* Always check system information carefully, raise error if a certain configuration can’t be done to prevent faulty binaries.

* Always, check required libraries to continue build process, raise error if not found.
    ```
    if(Boost_FOUND)
        message("Boost Found")
    else()
        error("Boost Not Found")
    endif()
    ```

