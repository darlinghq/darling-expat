project(expat)

cmake_minimum_required(VERSION 2.4.0)
enable_language(C ASM)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -msse -msse2 -msse3 -w -nostdinc")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D__DARWIN_UNIX03 -fPIC -w")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -nostdlib -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")

add_definitions(-DHAVE_EXPAT_CONFIG_H)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/)
include_directories(${DARLING_TOP_DIRECTORY}/platform-include)

set(expat_sources
	expat/lib/xmlparse.c
	expat/lib/xmlrole.c
	expat/lib/xmltok.c
	expat/lib/xmltok_impl.c
	expat/lib/xmltok_ns.c
)

SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/darling")
SET(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE) 
SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

add_library(expat SHARED ${expat_sources})
target_link_libraries(expat system)

install(TARGETS expat DESTINATION ${CMAKE_INSTALL_LIBDIR}/darling)
