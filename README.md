# Steps-for-Porting-ROS2-onto-QNX
Provides the Detailed Steps For Porting ROS2 onto QNX
Steps for Porting ROs2 onto QNX

Basically porting can be done in two ways, Below are the ways to do it
- Traditional cmake changes to the existing ROS2 source repository
- By creating the eclipse project using momentics ide for QNX


Steps to port ROS2 onto QNX using momentics ide

ROS2 packages are mentioned below with details to compile :

- Tinyxml2: 
Tinyxml2 is third-party source code, tinyxml2 is C++ library should be built using the GNU C++ Library with the compiler options set to "-std=gnu++14 -stdlib=libstdc++"

- Fast-CDR: 
Fast-CDR is DDS layer which is responsible for Serialization and de-serialization of ROS-Messages, Fast-CDR is C++ library should be built using the GNU C++ Library with the compiler options set to "-std=gnu++14 -stdlib=libstdc++" 

- Fast-RTPS: 
 Fast-RTPS is DDS layer implementation mainly responsible for Publishing and Subscribing of a Data, Fast-RTPS is C++ library should be built using the GNU C++ Library with the compiler options set to "-std=gnu++14 -stdlib=libstdc++" along with this option we need need provide an option for Asio C++ library to use non boost version of Asio "-DASIO_STANDALONE", Fast-RTPS library depends on Asio,Tinyxml2 and Fast-CDR, Asio, which is a set header files which contains the asynchronous model of modern C++ approach 

- RCUTILS: 
 RCUTILS is a C-Library consisting of some Utilities for Logging, string parsers and error handling. RCUTILS is C-Library should be built by linking "-stdlib=libstdc++" 

- ROSIDL_GENERATOR_C, ROSIDL_TYPESUPPORT_C, ROSIDL_TYPESUPPORT_CPP, ROSIDL_TYPESUPPORT_INTROSPECTION_C, ROSIDL_TYPESUPPORT_INTROSPECTION_CPP, BUILTIN_INTERFACES__ROSIDL_GENERATOR_C, BUILTIN_INTERFACES__ROSIDL_TYPESUPPORT_C, BUILTIN_INTERFACES__ROSIDL_TYPESUPPORT_CPP, BUILTIN_INTERFACES__ROSIDL_TYPESUPPORT_INTROSPECTION_C, BUILTIN_INTERFACES__ROSIDL_TYPESUPPORT_INTROSPECTION_CPP, ROSGRAPH_MSGS__ROSIDL_GENERATOR_C, ROSGRAPH_MSGS__ROSIDL_TYPESUPPORT_C, ROSGRAPH_MSGS__ROSIDL_TYPESUPPORT_CPP, ROSGRAPH_MSGS__ROSIDL_TYPESUPPORT_INTROSPECTION_C, ROSGRAPH_MSGS__ROSIDL_TYPESUPPORT_INTROSPECTION_CPP, RCL_INTERFACES__ROSIDL_GENERATOR_C, RCL_INTERFACES__ROSIDL_TYPESUPPORT_C, RCL_INTERFACES__ROSIDL_TYPESUPPORT_CPP, RCL_INTERFACES__ROSIDL_TYPESUPPORT_INTROSPECTION_C, RCL_INTERFACES__ROSIDL_TYPESUPPORT_INTROSPECTION_CPP
 All the above packages names the different type of libraries, distinguishing it as a C or C++ library. All C-Library should be built by linking "-stdlib=libstdc++" and C++ with the compiler options set to "-std=gnu++14 -stdlib=libstdc++" above packages are the required to as they contain some of builtin-interfaces and other basic data-types 

- RMW: 
 RMW is a C-Library acting as an interface between DDS layer and upper ROS layers. RMW is C-Library should be built by linking "-stdlib=libstdc++" 

- YAML: 
 YAML is a C-Library used for parsing YAML files. YAML is C-Library should be built by linking "-stdlib=libstdc++" and other compiler options are  -Wno-unused-value -DYAML_VERSION_MAJOR=0 -DYAML_VERSION_MINOR=1 -DYAML_VERSION_PATCH=7 -Wno-unused-but-set-variable -DYAML_VERSION_STRING='"${YAML_VERSION_MAJOR}.${YAML_VERSION_MINOR}.${YAML_VERSION_PATCH}"' 

- RCL: 
 RCL is a C-Library which acts as an C-interface for building ROS applications. RCL is C-Library should be built by linking "-stdlib=libstdc++" and providing the package name " -DROS_PACKAGE_NAME='"rcl"' " 

- RCL_YAML_PARAM_PARSER: 
 RCL_YAML_PARAM_PARSER is a C-Library for parsing RCL yaml files. RCUTILS is C-Library should be built by linking "-stdlib=libstdc++"  

- RMW_FASTRTPS_CPP: 
 Implemetation of ROS2 Middleware interface using DDS's Fast-RTPS. RMW_FASTRTPS_CPP is C++ library should be built using the GNU C++ Library with the compiler options set to "-std=gnu++14 -stdlib=libstdc++"  

- STD_MSGS__ROSIDL_TYPESUPPORT_C, STD_MSGS__ROSIDL_TYPESUPPORT_CPP,STD_MSGS__ROSIDL_TYPESUPPORT_INTROSPECTION_C, STD_MSGS__ROSIDL_TYPESUPPORT_INTROSPECTION_CPP
 STD_MSGS are basic data types used, All the above packages names the different type of libraries, distinguishing it as a C or C++ library. All C-Library should be built by linking "-stdlib=libstdc++" and C++ with the compiler options set to "-std=gnu++14 -stdlib=libstdc++" above packages are the required to as they contain basic data-types 

- RCLCPP: 
 RCLCPP is an C++ library acting as an Interface for developing C++ applications. RCLCPP is C++ library should be built using the GNU C++ Library with the compiler options set to "-std=gnu++14 -stdlib=libstdc++" 
 Some common header files in .h and .hpp format which needs to placed in respective folder with packages names ROSIDL_GENERATOR_CPP, ROSGRAPH_MSGS__ROSIDL_GENERATOR_CPP, STD_MSGS__ROSIDL_GENERATOR_CPP, RCL_INTERFACES__ROSIDL_GENERATOR_CPP, BUILTIN_INTERFACES__ROSIDL_GENERATOR_CPP, Similar kind of header files will get added based on our custom data-type package definitions

- Once building these packages we build and execute the ROS2  examples like talker and Listener

 Rules for Compiling these ROS2  packages
 ROS2 packages source code is combination of C and C++ Language 
 Most of the C++ features  used in ROS2  uses C++11 and C++14 features 
 Compile C supported packages by linking C++ library "libstdc++" 
 C++ supported packages should be compiled using GNU C++ library, using the compiler options "-std=gnu++14 -stdlib=libstdc++"  
 Build the ROS2 packages in the same order as listed above \ROS2 packages can be built in both release and debug mode, by making a simple change inside Makefile 
 If Packages need to be built using debug option then set BUILD_PROFILE ?= debug 
 If Packages need to be built in  release mode then set BUILD_PROFILE ?= release 

- Steps to Build Custom ROS data-types for QNX
 When Building custom ROS data-types there will six source code folder with name "CUSTOM_MSGS__ROSIDL_GENERATOR_C", CUSTOM_MSGS__ROSIDL_GENERATOR_CPP", "CUSTOM__MSGS__ROSIDL_TYPESUPPORT_C", CUSTOM_MSGS__ROSIDL_TYPESUPPORT_CPP", "CUSTOM_MSGS__ROSIDL_TYPESUPPORT_INTROSPECTION_C", CUSTOM_MSGS__ROSIDL_TYPESUPPORT_INTROSPECTION_CPP", "CUSTOM_MSGS__ROSIDL_GENERATOR_CPP" folder contains the header files only, Remaining Source code folder should be built with respective shared library 
