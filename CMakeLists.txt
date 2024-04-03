cmake_minimum_required(VERSION 3.12)

# define project metadata

project(Template LANGUAGES CXX
                DESCRIPTION "Template for C++ projects"
                )

# setting required language standard

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# enable export of compile commands to compile_commands.json

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
#set(CMAKE_CXX_STANDARD_INCLUDE_DIRECTORIES ${CMAKE_CXX_IMPLICIT_INCLUDE_DIRECTORIES})

# add strict compile options (warnings become errors; respecting the language standard)

if(MSVC)
    add_compile_options(-W4 -WX)
elseif(UNIX)
    add_compile_options(-Werror -Wall -pedantic)
endif()

# configure CMAKE_BUILD_TYPE=DEBUG to use AddressSanitizer (ASAN) for supported compilers

set(FLAG "-fsanitize=address")
include(CheckCXXCompilerFlag)
set(CMAKE_REQUIRED_FLAGS ${FLAG})
unset(COMPILER_SUPPORTS_ASAN)
check_cxx_compiler_flag(${FLAG} COMPILER_SUPPORTS_ASAN)

if (COMPILER_SUPPORTS_ASAN)
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG_INIT} ${FLAG}" CACHE STRING "" FORCE)
    set(CMAKE_EXE_LINKER_FLAGS_DEBUG "${CMAKE_EXE_LINKER_FLAGS_DEBUG_INIT} ${FLAG}" CACHE STRING "" FORCE)
    set(CMAKE_SHARED_LINKER_FLAGS_DEBUG "${CMAKE_SHARED_LINKER_FLAGS_DEBUG_INIT} ${FLAG}" CACHE STRING "" FORCE)
endif()

# enable CTest

# enable_testing()


# including targets

add_subdirectory(src/hello_world)

# add executable target

add_executable(${PROJECT_NAME})
target_sources(${PROJECT_NAME} PRIVATE src/main.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE HelloWorldLib)

# build documentation (requires doxygen)

option(BUILD_DOC "Build documentation" OFF)
if(BUILD_DOC)
    add_subdirectory(doxygen)
endif()
