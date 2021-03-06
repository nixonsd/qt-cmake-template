cmake_minimum_required(VERSION 3.5)

project(test LANGUAGES CXX)

add_definitions(-DUNICODE -D_UNICODE)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY bin)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# QtCreator supports the following variables for Android, which are identical to qmake Android variables.
# Check http://doc.qt.io/qt-5/deployment-android.html for more information.
# They need to be set before the find_package(Qt5 ...) call.

#if(ANDROID)
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
#    if (ANDROID_ABI STREQUAL "armeabi-v7a")
#        set(ANDROID_EXTRA_LIBS
#            ${CMAKE_CURRENT_SOURCE_DIR}/path/to/libcrypto.so
#            ${CMAKE_CURRENT_SOURCE_DIR}/path/to/libssl.so)
#    endif()
#endif()

find_package(Qt5 COMPONENTS Widgets REQUIRED)

# Inclusion of code
set(VIEWS_DIR "${CMAKE_SOURCE_DIR}/views")
set(HEADERS_DIR "${CMAKE_SOURCE_DIR}/headers")
set(SOURCES_DIR "${CMAKE_SOURCE_DIR}/sources")
set(RESOURCES_DIR "${CMAKE_SOURCE_DIR}/resources")

include_directories(${VIEWS_DIR})
include_directories(${HEADERS_DIR})
include_directories(${SOURCES_DIR})

file(GLOB_RECURSE SOURCES
  "${VIEWS_DIR}/*.ui"
  "${HEADERS_DIR}/*.h"
  "${SOURCES_DIR}/*.cpp"
)

set(CMAKE_AUTOUIC_SEARCH_PATHS ${CMAKE_AUTOUIC_SEARCH_PATHS} ${VIEWS_DIR})

if (WIN32)
	add_executable(test WIN32 ${SOURCES})
elseif(UNIX)
	add_executable(test ${SOURCES})
endif()

# Include QT libraries
target_link_libraries(test PRIVATE Qt5::Widgets)

# Add the target includes
target_include_directories(test PRIVATE ${VIEWS_DIR})
target_include_directories(test PRIVATE ${HEADERS_DIR})
target_include_directories(test PRIVATE ${SOURCES_DIR})