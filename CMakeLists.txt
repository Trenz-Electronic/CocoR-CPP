cmake_minimum_required(VERSION 3.5)
project(cococpp CXX)
set(PACKAGE_AUTHOR "Andrei Errapart")

include(GNUInstallDirs)

# Data for packaging.
set(CPACK_PACKAGE_VERSION_MAJOR 1)
set(CPACK_PACKAGE_VERSION_MINOR 0)
set(CPACK_PACKAGE_VERSION_PATCH 0)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "The Compiler Generator Coco/R for C++"
    CACHE STRING "Coco/R is a compiler generator for C++, which takes an attributed grammar of a source language and generates a scanner and a parser for this language."
)
set(CPACK_PACKAGE_VENDOR "Trenz Electronic GmbH")
set(CPACK_PACKAGE_CONTACT "a.errapart@trenz-electronic.de")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Andrei Errapart <${CPACK_PACKAGE_CONTACT}>")
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/GPL_V2")
set(CPACK_RESOURCE_FILE_README "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
set(CPACK_DEBIAN_FILE_NAME DEB-DEFAULT) # otherwise we will get _Linux.deb file and not _amd64.deb.
set(CPACK_STRIP_FILES YES)
set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS YES)


# Requirements
find_package(PkgConfig REQUIRED) # for the pkg_check_modules to work

# libcococpp
file(GLOB_RECURSE cococpp_files "src/*.cpp")

# cococpp
add_executable(coco-cpp ${cococpp_files})
target_compile_features(coco-cpp PUBLIC cxx_std_20) # Require at least that version of C++.
target_link_libraries(coco-cpp)
install(TARGETS coco-cpp RUNTIME)
file(GLOB_RECURSE frame_files "src/*.frame")
install(FILES ${frame_files} DESTINATION "${CMAKE_INSTALL_DATADIR}/cococpp")


# Debian package
include(CPack)

