cmake_minimum_required(VERSION 3.10)
project(CQF)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 11)

# Set the source and header files
set(SRC_FILES
    src/gqf.c
    src/gqf_file.c
    src/hashutil.c
    src/partitioned_counter.c
)

set(HEADER_FILES
    include/gqf.h
    include/gqf_file.h
    include/hashutil.h
    include/partitioned_counter.h
)

# Set the include directories
include_directories(include)

# Set the build options
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug)
endif()

set(CMAKE_C_FLAGS_DEBUG "-g")
set(CMAKE_CXX_FLAGS_DEBUG "-g")
set(CMAKE_C_FLAGS_RELEASE "-Ofast")
set(CMAKE_CXX_FLAGS_RELEASE "-Ofast")

if(NOT DEFINED SSE4_2)
  set(SSE4_2 ON)
endif()

if(SSE4_2)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -msse4.2 -D__SSE4_2_")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -msse4.2 -D__SSE4_2_")
endif()

if(NOT DEFINED PROFILE)
  set(PROFILE OFF)
endif()

if(PROFILE)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pg -no-pie")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pg -no-pie")
endif()

# Create the target executable
# add_executable(cqf ${SRC_FILES})

# # Link the required libraries
# target_link_libraries(cqf pthread ssl crypto m)

add_library(cqf STATIC ${SRC_FILES})
target_link_libraries(cqf PRIVATE pthread ssl crypto m itm)