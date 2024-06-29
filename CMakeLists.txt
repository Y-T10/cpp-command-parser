cmake_minimum_required(VERSION 3.27.0...3.29.6)

if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

set(SUB_PROJECT OFF)
if (CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
        set(SUB_PROJECT ON)
endif()

option(ENABLE_EXAMPLES "Enable building example programs." ${SUB_PROJECT})
option(ENABLE_TESTS "Enable Building tests." ${SUB_PROJECT})

project(CppCommandParser
        VERSION 0.0.1 
        LANGUAGES CXX
)

add_compile_options(
        -Wcast-align
        -Wpedantic
        -Wcast-qual
        -Wconversion
        -Wctor-dtor-privacy
        -Wnon-virtual-dtor
        -Wmissing-include-dirs
        -Wdisabled-optimization
        -Winit-self
        -Wnon-virtual-dtor
        -Wold-style-cast
        -Woverloaded-virtual
        -Wparentheses
        -Wredundant-decls
        -Wshadow
        -Wsign-promo
        -Wstrict-aliasing
        -Wall
        -Wextra
        -Werror
)


add_library(command_parser INTERFACE)
add_library(command_parser::command_parser ALIAS command_parser)
target_include_directories(command_parser INTERFACE include)
target_compile_features(command_parser PUBLIC cxx_std_17)

if (${ENABLE_EXAMPLES})
        add_executable(cpp_command_parser_sample example_main.cpp)
        target_link_libraries(cpp_command_parser_sample PUBLIC command_parser)
        target_compile_features(cpp_command_parser_sample PRIVATE cxx_std_17)
endif()

if (${ENABLE_TESTS})
        add_subdirectory(external)
        enable_testing()
        add_subdirectory(test)
endif()
