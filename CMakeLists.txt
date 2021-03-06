##########################################################################################
#                                                                                        
#  88888888ba               88                                ad888888b,  88888888ba,    
#  88      "8b              88                         ,d    d8"     "88  88      `"8b   
#  88      ,8P              88                         88            a8P  88        `8b  
#  88aaaaaa8P'  ,adPPYba,   88,dPPYba,    ,adPPYba,  MM88MMM      aad8"   88         88  
#  88""""88'   a8"     "8a  88P'    "8a  a8"     "8a   88         ""Y8,   88         88  
#  88    `8b   8b       d8  88       d8  8b       d8   88            "8b  88         8P  
#  88     `8b  "8a,   ,a8"  88b,   ,a8"  "8a,   ,a8"   88,   Y8,     a88  88      .a8P   
#  88      `8b  `"YbbdP"'   8Y"Ybbd8"'    `"YbbdP"'    "Y888  "Y888888P'  88888888Y"'    
#
##########################################################################################
# Part of the Robot3D simulator, a 3D robot simulator for modular robotics. This makefile
# is part of a cmake build system. The simulator is developed within the FP7 European
# projects Replicator and Symbrion. The namespace "sr" is an abbreviation of those project
# names. The code falls under the Lesser General Public License (LGPL GNU v3).

# Author:	 Anne C. van Rossum (Almende B.V.)
# Date: 	 Oct 28, 2010
#
# ASCII Art generated by: http://www.network-science.de/ascii/ with font "univers".
#
# Copyright © 2010 Anne van Rossum <anne@almende.com>
##########################################################################################

IF(WIN32)
cmake_minimum_required(VERSION 2.6)
ELSE(WIN32)
cmake_minimum_required(VERSION 2.4)
ENDIF(WIN32)

# The directory with all the FindXXX modules
SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

# The macro below forces the build directory to be different from source directory:
INCLUDE( MacroEnsureOutOfSourceBuild )
MACRO_ENSURE_OUT_OF_SOURCE_BUILD(
	"${PROJECT_NAME} requires an out of source build."
)

SET(PROJECT_NAME "Reservoir")

# Start a project.
PROJECT(${PROJECT_NAME})

# Find packages
#FIND_PACKAGE(YARP REQUIRED)

# Header files
#INCLUDE_DIRECTORIES(${YARP_INCLUDE_DIRS})

INCLUDE_DIRECTORIES(thd)

# Shared libraries
#SET(LIBS ${LIBS} ${YARP_LIBRARIES})

# Some debug information
MESSAGE("${PROJECT_NAME} is using CXX flags: ${CMAKE_CXX_FLAGS}")
MESSAGE ("Libraries included: ${LIBS}")

# Search for source code.
FILE(GLOB folder_source src/*.cpp src/*.cc src/*.c thd/inv/*.cpp thd/eigenvalues/*.cpp)
FILE(GLOB folder_header inc/*.h thd/inv/*.h thd/eigenvalues/*.h src/*.h)

SOURCE_GROUP("Source Files" FILES ${folder_source})
SOURCE_GROUP("Header Files" FILES ${folder_header})

# Automatically add include directories if needed.
FOREACH(header_file ${folder_header})
  GET_FILENAME_COMPONENT(p ${header_file} PATH)
  INCLUDE_DIRECTORIES(${p})
ENDFOREACH(header_file ${folder_header})

# Testing
# enable_testing()
# add_subdirectory(harness)

# Set up our main executable.
IF (folder_source)
   ADD_EXECUTABLE(${PROJECT_NAME} ${folder_source} ${folder_header})
   TARGET_LINK_LIBRARIES(${PROJECT_NAME} ${LIBS})
   install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)   
ELSE (folder_source)
  MESSAGE(FATAL_ERROR "No source code files found. Please add something")
ENDIF (folder_source)

