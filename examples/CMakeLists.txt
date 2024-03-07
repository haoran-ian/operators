cmake_minimum_required(VERSION 3.17)

project(cec_test)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)

include_directories(/data/hyin/local/include)

set(ioh_DIR "/data/hyin/local/lib64/cmake/ioh")
find_package(ioh REQUIRED)

add_executable(cec_test init_population.cpp)
target_link_libraries(cec_test ioh::ioh)
