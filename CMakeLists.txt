cmake_minimum_required(VERSION 3.9)
cmake_policy(SET CMP0077 NEW)
cmake_policy(SET CMP0079 NEW)

# Set version
if(CMAKE_BUILD_TYPE STREQUAL "Release")
    execute_process(COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/version.sh VERSION ${INCREMENT_VERSION}
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        OUTPUT_VARIABLE CUSTOM_VERSION)
else()
    set(CMAKE_BUILD_TYPE Debug)
    set(CUSTOM_VERSION 0.0.0)
endif()

# Project information
project(PROJECT_NAME VERSION ${CUSTOM_VERSION} DESCRIPTION "Bitcoin exchange project")

# CMake options
set(CMAKE_C_STANDARD 17)
set(CMAKE_CXX_STANDARD 20)
# GNU standard installation directories
include(GNUInstallDirs)

# Find libcurl
find_package(CURL REQUIRED)

# Include directories
include_directories(${CURL_INCLUDE_DIRS})
include_directories(${PROJECT_SOURCE_DIR}/include) # For nlohmann/json

# Source files
set(SOURCES
    src/bitcoin.cpp
)

# Target definitions
add_executable(${PROJECT_NAME} ${SOURCES})

# Include directories
target_include_directories(${PROJECT_NAME} PUBLIC ${CURL_INCLUDE_DIRS})
target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/include)

# Link libraries
target_link_libraries(${PROJECT_NAME} PRIVATE ${CURL_LIBRARIES})

# Install
install(TARGETS ${PROJECT_NAME}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR} # bin
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} # lib
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}) # include
