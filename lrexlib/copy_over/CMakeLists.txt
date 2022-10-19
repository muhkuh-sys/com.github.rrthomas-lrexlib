cmake_minimum_required(VERSION 3.7)

PROJECT("com.github.rrthomas-lrexlib")

#-----------------------------------------------------------------------------
#
# Build the lrexlib_pcre module.
#
ADD_LIBRARY(TARGET_lrexlib_pcre MODULE
            src/common.c
            src/pcre/lpcre.c
            src/pcre/lpcre_f.c)

TARGET_COMPILE_DEFINITIONS(TARGET_lrexlib_pcre
                           PRIVATE VERSION="${LREXLIB_VERSION}"
                                   PCRE_STATIC)

# Add the includes and library for LUA and PCRE.
TARGET_INCLUDE_DIRECTORIES(TARGET_lrexlib_pcre
                           PRIVATE ${LUA_INCLUDE_DIR} ${LIBPCRE_INCLUDE_DIRECTORIES})
TARGET_LINK_LIBRARIES(TARGET_lrexlib_pcre
                      PRIVATE ${LUA_LIBRARIES} ${LIBPCRE_LIBRARY})

# On MinGW link all compiler libraries static.
IF((${CMAKE_SYSTEM_NAME} STREQUAL "Windows") AND (${CMAKE_COMPILER_IS_GNUCC}))
	SET_PROPERTY(TARGET TARGET_lrexlib_pcre PROPERTY LINK_FLAGS "-static-libgcc -static-libstdc++")
ENDIF((${CMAKE_SYSTEM_NAME} STREQUAL "Windows") AND (${CMAKE_COMPILER_IS_GNUCC}))

# Set the name of the output file to "rex_pcre" without any prefix like "lib".
SET_TARGET_PROPERTIES(TARGET_lrexlib_pcre PROPERTIES PREFIX "" OUTPUT_NAME "rex_pcre")

INSTALL(TARGETS TARGET_lrexlib_pcre
        LIBRARY DESTINATION ${INSTALL_DIR_LUA_MODULES})
