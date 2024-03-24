cmake_minimum_required(VERSION 3.10)
project(libiptcdata VERSION 1.0.5 LANGUAGES C)

# Compiler flags
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -Wall")

# Dependencies
find_package(iconv REQUIRED)
find_package(gettext REQUIRED)

# Include directories
include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/libiptcdata)

# Define the source files for the library
set(SOURCES
    libiptcdata/iptc-data.c
    libiptcdata/iptc-dataset.c
    libiptcdata/iptc-jpeg.c
    libiptcdata/iptc-log.c
    libiptcdata/iptc-mem.c
    libiptcdata/iptc-tag.c
    libiptcdata/iptc-utils.c
)

# Define the library
add_library(iptcdata SHARED ${SOURCES})
set_target_properties(iptcdata PROPERTIES VERSION ${PROJECT_VERSION})
set_target_properties(iptcdata PROPERTIES SOVERSION 1)
set_target_properties(iptcdata PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS TRUE)

# Define include directories for the target
target_include_directories(iptcdata PUBLIC $<INSTALL_INTERFACE:include> $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)

# Installation rules for the library
install(TARGETS iptcdata
        EXPORT iptcdataTargets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin
        INCLUDES DESTINATION include
)

# Installation rules for the headers
install(FILES
    libiptcdata/iptc-data.h
    libiptcdata/iptc-dataset.h
    libiptcdata/iptc-jpeg.h
    libiptcdata/iptc-log.h
    libiptcdata/iptc-mem.h
    libiptcdata/iptc-tag.h
    libiptcdata/iptc-utils.h
    DESTINATION include/libiptcdata
)

# Generate and install the pkg-config file
include(FindPkgConfig)
if(PKG_CONFIG_FOUND)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/libiptcdata/libiptcdata.pc.in ${CMAKE_CURRENT_BINARY_DIR}/libiptcdata.pc @ONLY)
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/libiptcdata.pc DESTINATION lib/pkgconfig)
endif()

# Additional configurations here...

