cmake_minimum_required(VERSION 3.12)

# Add as library
add_library(HelloWorldLib STATIC)
target_sources(HelloWorldLib PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/hello_world.cpp)
target_include_directories(HelloWorldLib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})