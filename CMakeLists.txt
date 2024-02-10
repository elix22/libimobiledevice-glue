cmake_minimum_required(VERSION 3.0)
project(libimobiledevice-glue)


set(CMAKE_CXX_STANDARD 20)

if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    add_definitions(-DHAVE_STRNDUP -DPACKAGE_VERSION="v1.3")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    add_definitions(-DHAVE_STRNDUP -DHAVE_GETIFADDRS -DPACKAGE_VERSION="v1.3")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    add_definitions(-DHAVE_STRNDUP -DHAVE_GETIFADDRS -DPACKAGE_VERSION="v1.3")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++ -x objective-c++ ")
endif()

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../libplist ${CMAKE_CURRENT_SOURCE_DIR}/../libplist/build)

# Collect all .cpp files recursively
file(GLOB_RECURSE LIBIMOBILEDEVICE_GLUE_SOURCES  src/*.c src/*.h)

add_library(libimobiledevice SHARED ${LIBIMOBILEDEVICE_GLUE_SOURCES})

target_include_directories(libimobiledevice PRIVATE include src ../libplist/include)
# Link libplist
target_link_libraries(libimobiledevice PRIVATE libplist)
