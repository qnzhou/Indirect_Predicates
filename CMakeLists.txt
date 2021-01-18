cmake_minimum_required(VERSION 3.10)

# set the project name
project(indirectPredicates)

add_library(${PROJECT_NAME} STATIC
    implicit_point.cpp
    numerics.cpp
    predicates/indirect_predicates.cpp
    predicates/hand_optimized_predicates.cpp
)

# Compiler-specific options
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
# grant IEEE 754 compliance
    target_compile_options(${PROJECT_NAME} PRIVATE -frounding-math)
# use intrinsic functions (CHECK WHAT TO DO FOR GCC !!!!!!!!)
#	target_compile_options(${PROJECT_NAME} PUBLIC "/Oi")
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
# grant IEEE 754 compliance
    target_compile_options(${PROJECT_NAME} PRIVATE "/fp:strict")
# use intrinsic functions
    target_compile_options(${PROJECT_NAME} PRIVATE "/Oi")
# turn off annoying warnings
    target_compile_options(${PROJECT_NAME} PRIVATE "/D _CRT_SECURE_NO_WARNINGS")
elseif("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang" OR "${CMAKE_CXX_COMPILER_ID}" STREQUAL "AppleClang")
    target_compile_options(${PROJECT_NAME} PRIVATE "-O0")
endif()

# Public include directory
target_include_directories(${PROJECT_NAME} PUBLIC .)
target_include_directories(${PROJECT_NAME} PUBLIC predicates)

# add the executable
add_executable(indirect_predicates_test test.cpp)
target_link_libraries(indirect_predicates_test PRIVATE ${PROJECT_NAME})

