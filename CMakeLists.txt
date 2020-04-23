cmake_minimum_required(VERSION 3.10)

set(PROJ_NAME "helloworld")
project(${PROJ_NAME} VERSION 1.0.0 LANGUAGES CXX)

# Add option to build in Debug or Release version
set(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "Configs" FORCE)
set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS  ${CMAKE_CONFIGURATION_TYPES})

# Add postfix to debug output
SET(CMAKE_DEBUG_POSTFIX "d")

# Where to place output
# set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/bin)
# set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
# set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}) 

# Change include path
include_directories(${PROJECT_SOURCE_DIR}/inc)

# Add options to compiler
# set(CMAKE_CXX_COMPILER clang++)
# set(CMAKE_CXX_COMPILER g++)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DUSE_UTF8=1")
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Generate compile time info (just example code)
# configure_file("${CMAKE_SOURCE_DIR}/inc/jim_info.h.in" "${CMAKE_SOURCE_DIR}/inc/jim_info.h")

# Build static library hellolib
add_library(hellolib STATIC
${CMAKE_SOURCE_DIR}/hellolib.cpp
  )
# ${CMAKE_SOURCE_DIR}/portabilty/jim-tty.cpp
# ${CMAKE_SOURCE_DIR}/portabilty/jim-zlib.cpp


# Build shared library hellolibso
add_library(hellolibso SHARED
${CMAKE_SOURCE_DIR}/hellolib.cpp
	)

# TODO How to build debug jimshd?

# TODO Use library in build of jimshpp
# target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# Build executable helloworld
add_executable(helloworld 
${CMAKE_SOURCE_DIR}/hellolib.cpp

${CMAKE_SOURCE_DIR}/helloworld.cpp
	)

set_target_properties(helloworld PROPERTIES DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX})

# install(TARGETS helloworld)

enable_testing()

# does the application run
add_test(NAME Runs COMMAND helloworld)

# define a function to simplify adding tests
function(do_test target arg result)
  add_test(NAME Comp${arg} COMMAND ${target} ${arg})
  set_tests_properties(Comp${arg}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result}
    )
endfunction(do_test)

# do_test(Tutorial 4 "4 is 2")


