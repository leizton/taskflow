# eigen package 
if(NOT DEFINED EIGEN_ROOT)
  set(EIGEN_ROOT ${PROJECT_SOURCE_DIR}/3rd-party/eigen-3.3.7)
endif()
include_directories(${EIGEN_ROOT})

# find OpenMP package
include(FindOpenMP)

if(NOT OpenMP_CXX_FOUND)
  message(FATAL_ERROR "OpenMP not found")
endif()
  
message(STATUS "OpenMP_VERSION: ${OpenMP_VERSION}")
message(STATUS "OpenMP_CXX_FLAGS: ${OpenMP_CXX_FLAGS}")
message(STATUS "OpenMP_CXX_LIBRARIES: ${OpenMP_CXX_LIBRARIES}")

# tbb package
if(NOT DEFINED TBB_ROOT)
  set(TBB_ROOT ${TF_3RD_PARTY_DIR}/tbb)
endif()

include(${TBB_ROOT}/cmake/TBBBuild.cmake)
tbb_build(TBB_ROOT ${TBB_ROOT} CONFIG_DIR TBB_DIR MAKE_ARGS tbb_cpf=1)
#tbb_build(TBB_ROOT ${TBB_ROOT} CONFIG_DIR TBB_DIR)
#cmake_policy(PUSH)
#cmake_policy(SET CMP0074 NEW)
#find_package(TBB REQUIRED tbb_preview)
find_package(TBB REQUIRED tbb_preview)
#cmake_policy(POP)

## benchmark 1: wavefront computing
add_executable(
  wavefront 
  ${TF_BENCHMARK_DIR}/wavefront/main.cpp
  ${TF_BENCHMARK_DIR}/wavefront/omp.cpp
  ${TF_BENCHMARK_DIR}/wavefront/tbb.cpp
  ${TF_BENCHMARK_DIR}/wavefront/seq.cpp
  ${TF_BENCHMARK_DIR}/wavefront/taskflow.cpp
)
target_include_directories(wavefront PRIVATE ${PROJECT_SOURCE_DIR}/3rd-party/CLI11)
target_link_libraries(
  wavefront 
  ${PROJECT_NAME} 
  ${TBB_IMPORTED_TARGETS} 
  ${OpenMP_CXX_LIBRARIES} 
  tf::default_settings
)
set_target_properties(wavefront PROPERTIES COMPILE_FLAGS ${OpenMP_CXX_FLAGS})

## benchmark 2: graph traversal
add_executable(
  graph_traversal 
  ${TF_BENCHMARK_DIR}/graph_traversal/main.cpp
  ${TF_BENCHMARK_DIR}/graph_traversal/omp.cpp
  ${TF_BENCHMARK_DIR}/graph_traversal/tbb.cpp
  ${TF_BENCHMARK_DIR}/graph_traversal/seq.cpp
  ${TF_BENCHMARK_DIR}/graph_traversal/taskflow.cpp
)
target_include_directories(graph_traversal PRIVATE ${PROJECT_SOURCE_DIR}/3rd-party/CLI11)
target_link_libraries(
  graph_traversal 
  ${PROJECT_NAME} 
  ${TBB_IMPORTED_TARGETS} 
  ${OpenMP_CXX_LIBRARIES} 
  tf::default_settings
)
set_target_properties(graph_traversal PROPERTIES COMPILE_FLAGS ${OpenMP_CXX_FLAGS})

## benchmark 3: binary_tree
add_executable(
  binary_tree 
  ${TF_BENCHMARK_DIR}/binary_tree/main.cpp
  ${TF_BENCHMARK_DIR}/binary_tree/tbb.cpp
  ${TF_BENCHMARK_DIR}/binary_tree/omp.cpp
  ${TF_BENCHMARK_DIR}/binary_tree/taskflow.cpp
)
target_include_directories(binary_tree PRIVATE ${PROJECT_SOURCE_DIR}/3rd-party/CLI11)
target_link_libraries(
  binary_tree 
  ${PROJECT_NAME} 
  ${TBB_IMPORTED_TARGETS} 
  ${OpenMP_CXX_LIBRARIES} 
  tf::default_settings
)
set_target_properties(binary_tree PROPERTIES COMPILE_FLAGS ${OpenMP_CXX_FLAGS})

## benchmark 4: linear_chain
add_executable(
  linear_chain
  ${TF_BENCHMARK_DIR}/linear_chain/main.cpp
  ${TF_BENCHMARK_DIR}/linear_chain/tbb.cpp
  ${TF_BENCHMARK_DIR}/linear_chain/omp.cpp
  ${TF_BENCHMARK_DIR}/linear_chain/taskflow.cpp
)
target_include_directories(linear_chain PRIVATE ${PROJECT_SOURCE_DIR}/3rd-party/CLI11)
target_link_libraries(
  linear_chain 
  ${PROJECT_NAME} 
  ${TBB_IMPORTED_TARGETS} 
  ${OpenMP_CXX_LIBRARIES} 
  tf::default_settings
)
set_target_properties(linear_chain PROPERTIES COMPILE_FLAGS ${OpenMP_CXX_FLAGS})

## benchmark 5: MNIST
add_executable(
  mnist 
  ${TF_BENCHMARK_DIR}/mnist/main.cpp
  ${TF_BENCHMARK_DIR}/mnist/omp.cpp
  ${TF_BENCHMARK_DIR}/mnist/tbb.cpp
  ${TF_BENCHMARK_DIR}/mnist/seq.cpp
  ${TF_BENCHMARK_DIR}/mnist/taskflow.cpp
)
target_include_directories(mnist PRIVATE ${PROJECT_SOURCE_DIR}/3rd-party/CLI11)
target_link_libraries(
  mnist 
  ${PROJECT_NAME} 
  ${TBB_IMPORTED_TARGETS}
  ${OpenMP_CXX_LIBRARIES} 
  stdc++fs 
  tf::default_settings
)
set_target_properties(mnist PROPERTIES COMPILE_FLAGS ${OpenMP_CXX_FLAGS})

## benchmark 6: matrix multiplication
add_executable(
  matrix_multiplication 
  ${TF_BENCHMARK_DIR}/matrix_multiplication/main.cpp
  ${TF_BENCHMARK_DIR}/matrix_multiplication/omp.cpp
  ${TF_BENCHMARK_DIR}/matrix_multiplication/tbb.cpp
  ${TF_BENCHMARK_DIR}/matrix_multiplication/taskflow.cpp
)
target_include_directories(matrix_multiplication PRIVATE ${PROJECT_SOURCE_DIR}/3rd-party/CLI11)
target_link_libraries(
  matrix_multiplication 
  ${PROJECT_NAME} 
  ${TBB_IMPORTED_TARGETS} 
  ${OpenMP_CXX_LIBRARIES} 
  tf::default_settings
)
set_target_properties(matrix_multiplication PROPERTIES COMPILE_FLAGS ${OpenMP_CXX_FLAGS})

## benchmark 7: mandelbrot
add_executable(
  mandelbrot 
  ${TF_BENCHMARK_DIR}/mandelbrot/main.cpp
  ${TF_BENCHMARK_DIR}/mandelbrot/omp.cpp
  ${TF_BENCHMARK_DIR}/mandelbrot/tbb.cpp
  ${TF_BENCHMARK_DIR}/mandelbrot/taskflow.cpp
)
target_include_directories(mandelbrot PRIVATE ${PROJECT_SOURCE_DIR}/3rd-party/CLI11)
target_link_libraries(
  mandelbrot 
  ${PROJECT_NAME} 
  ${TBB_IMPORTED_TARGETS} 
  ${OpenMP_CXX_LIBRARIES} 
  tf::default_settings
)
set_target_properties(mandelbrot PROPERTIES COMPILE_FLAGS ${OpenMP_CXX_FLAGS})

## benchmark 8: black_scholes
add_executable(
  black_scholes
  ${TF_BENCHMARK_DIR}/black_scholes/main.cpp
  ${TF_BENCHMARK_DIR}/black_scholes/omp.cpp
  ${TF_BENCHMARK_DIR}/black_scholes/tbb.cpp
  ${TF_BENCHMARK_DIR}/black_scholes/taskflow.cpp
)
target_include_directories(black_scholes PRIVATE ${PROJECT_SOURCE_DIR}/3rd-party/CLI11)
target_link_libraries(
  black_scholes 
  ${PROJECT_NAME} 
  ${TBB_IMPORTED_TARGETS} 
  ${OpenMP_CXX_LIBRARIES} 
  tf::default_settings
)
set_target_properties(black_scholes PROPERTIES COMPILE_FLAGS ${OpenMP_CXX_FLAGS})

## benchmark 9: reduce_sum
add_executable(
  reduce_sum
  ${TF_BENCHMARK_DIR}/reduce_sum/main.cpp
  ${TF_BENCHMARK_DIR}/reduce_sum/omp.cpp
  ${TF_BENCHMARK_DIR}/reduce_sum/tbb.cpp
  ${TF_BENCHMARK_DIR}/reduce_sum/taskflow.cpp
)
target_include_directories(reduce_sum PRIVATE ${PROJECT_SOURCE_DIR}/3rd-party/CLI11)
target_link_libraries(
  reduce_sum 
  ${PROJECT_NAME} 
  ${TBB_IMPORTED_TARGETS} 
  ${OpenMP_CXX_LIBRARIES} 
  tf::default_settings
)
set_target_properties(reduce_sum PROPERTIES COMPILE_FLAGS ${OpenMP_CXX_FLAGS})


if(CMAKE_CUDA_COMPILER AND TF_BUILD_CUDA)
## cuda benchmark 1: heterogeneous traversal
add_executable(
  hetero_traversal 
  ${TF_BENCHMARK_DIR}/hetero_traversal/main.cu
  ${TF_BENCHMARK_DIR}/hetero_traversal/taskflow.cu
  ${TF_BENCHMARK_DIR}/hetero_traversal/tbb.cu
  ${TF_BENCHMARK_DIR}/hetero_traversal/omp.cu
)
target_include_directories(hetero_traversal PRIVATE ${PROJECT_SOURCE_DIR}/3rd-party/CLI11)
target_link_libraries(
  hetero_traversal 
  ${PROJECT_NAME} 
  Threads::Threads 
  ${TBB_IMPORTED_TARGETS} 
  ${OpenMP_CXX_LIBRARIES} 
  tf::default_settings
)
set_target_properties(hetero_traversal PROPERTIES COMPILE_FLAGS "-Xcompiler ${OpenMP_CXX_FLAGS}")
endif(CMAKE_CUDA_COMPILER AND TF_BUILD_CUDA)
