cmake_minimum_required(VERSION 2.8.12)
project(test_sparse_map)
enable_testing()

find_package(Boost 1.54.0 REQUIRED COMPONENTS unit_test_framework) 


set(TEST_EXECUTABLE "test_sparse_map")

add_executable("${TEST_EXECUTABLE}" "tests/main.cpp" 
                                    "tests/custom_allocator_tests.cpp"
                                    "tests/policy_tests.cpp"
                                    "tests/popcount_tests.cpp"
                                    "tests/sparse_map_tests.cpp"
                                    "tests/sparse_set_tests.cpp")
                                    
target_include_directories("${TEST_EXECUTABLE}" PRIVATE "${Boost_INCLUDE_DIRS}" ".") 
target_link_libraries("${TEST_EXECUTABLE}" ${Boost_LIBRARIES})

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR CMAKE_CXX_COMPILER_ID MATCHES "GNU")
    target_compile_options("${TEST_EXECUTABLE}" PRIVATE -std=c++11 -Werror -Wall -Wextra -Wold-style-cast -O3 -DTSL_DEBUG)
elseif(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
    target_compile_options("${TEST_EXECUTABLE}" PRIVATE /WX /W3 /DTSL_DEBUG)
endif()


add_test(NAME "all_tests" COMMAND "${TEST_EXECUTABLE}")
