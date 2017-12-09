# cmake_minimum_required(VERSION <specify CMake version here>)
project(kaldi)

cmake_minimum_required(VERSION 2.8)

#set(CMAKE_CXX_STANDARD 11)

#set(CMAKE_CXX_COMPILER /opt/intel/bin/icc)

add_compile_options(-std=c++11)

add_compile_options(-g -Wall -Winline -pipe -O2)
add_compile_options(-DHAVE_ATLAS)

include_directories(
        ./
        ../tools/openfst/include
        ../tools/ATLAS/include
)
link_directories(
        ${PROJECT_SOURCE_DIR}/cmake-build-debug
        /usr/lib/
        #/opt/intel/mkl/lib/intel64
)

aux_source_directory(./base SOURCE_FILES_BASE)
aux_source_directory(./util SOURCE_FILES_UTIL)
aux_source_directory(./cudamatrix SOURCE_FILES_CUDAMATRIX)
aux_source_directory(./matrix SOURCE_FILES_MATRIX)
aux_source_directory(./feat SOURCE_FILES_FEAT)
aux_source_directory(./nnet SOURCE_FILES_NNET)
aux_source_directory(./lat SOURCE_FILES_LAT)
aux_source_directory(./hmm SOURCE_FILES_HMM)
aux_source_directory(./tree SOURCE_FILES_TREE)
aux_source_directory(./thread SOURCE_FILES_THREAD)

add_library(k_base STATIC ${SOURCE_FILES_BASE})
add_library(k_util STATIC ${SOURCE_FILES_UTIL})
add_library(k_cudamatrix STATIC ${SOURCE_FILES_CUDAMATRIX})
add_library(k_matrix STATIC ${SOURCE_FILES_MATRIX})
add_library(k_feat STATIC ${SOURCE_FILES_FEAT})
add_library(k_lat STATIC ${SOURCE_FILES_LAT})
add_library(k_hmm STATIC ${SOURCE_FILES_HMM})
add_library(k_tree STATIC ${SOURCE_FILES_TREE})
add_library(k_thread STATIC ${SOURCE_FILES_THREAD})

add_library(k_nnet STATIC ${SOURCE_FILES_NNET})

set(SOURCE_FILES_NNETBIN_NNET_INFO nnetbin/nnet-info.cc)
add_executable(nnet-info ${SOURCE_FILES_NNETBIN_NNET_INFO})
target_link_libraries(nnet-info
        k_nnet.a
        k_cudamatrix.a
        k_lat.a
        k_hmm.a
        k_tree.a
        k_util.a
        k_thread.a
        k_matrix.a
        k_base.a

        libatlas.so.3
        libf77blas.so.3
        libcblas.so.3
        liblapack_atlas.so.3
        )



