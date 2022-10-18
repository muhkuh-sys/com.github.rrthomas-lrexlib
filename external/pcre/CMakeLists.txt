cmake_minimum_required(VERSION 3.2.3)

PROJECT("pcre")

INCLUDE(ExternalProject)

SET(VERSION_EXTERNAL "8.45")


#----------------------------------------------------------------------------
#
# Build the project.
#
SET(PRJ_CMAKE_ARGS "")
LIST(APPEND PRJ_CMAKE_ARGS "-DPCRE_BUILD_PCRECPP=OFF")
LIST(APPEND PRJ_CMAKE_ARGS "-DPCRE_BUILD_PCREGREP=OFF")
LIST(APPEND PRJ_CMAKE_ARGS "-DPCRE_BUILD_TESTS=OFF")
LIST(APPEND PRJ_CMAKE_ARGS "-DCMAKE_TOOLCHAIN_FILE=${CMAKE_TOOLCHAIN_FILE}")
LIST(APPEND PRJ_CMAKE_ARGS "-DCMAKE_INSTALL_PREFIX=''")

ExternalProject_Add(TARGET_libpcre
                    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/pcre
                    URL ${CMAKE_CURRENT_SOURCE_DIR}/pcre-${VERSION_EXTERNAL}.tar.bz2
                    URL_HASH SHA1=8463cc88b737e2c3ce7f041922a6ff0c5abbed34
                    PATCH_COMMAND "${PYTHON_INTERPRETER}" ${CMAKE_HOME_DIRECTORY}/cmake/tools/apply_patches.py --working-folder ${CMAKE_CURRENT_BINARY_DIR}/pcre/src/TARGET_libpcre --patch-folder ${CMAKE_CURRENT_SOURCE_DIR}/patches --strip 1
                    CMAKE_ARGS ${PRJ_CMAKE_ARGS}
                    INSTALL_COMMAND make install DESTDIR=${CMAKE_CURRENT_BINARY_DIR}/install
)
