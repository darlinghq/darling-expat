project(expat)

cmake_minimum_required(VERSION 2.4.0)
enable_language(C ASM)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -msse -msse2 -msse3 -w -nostdinc")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D__DARWIN_UNIX03 -fPIC -w")

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

set(DYLIB_INSTALL_NAME "/usr/lib/libexpat.1.dylib")
add_darling_library(expat SHARED ${expat_sources})
make_fat(expat)
target_link_libraries(expat system)
set_target_properties(expat PROPERTIES OUTPUT_NAME "expat.1")

InstallSymlink("libexpat.1.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/libexpat.dylib")

install(TARGETS expat DESTINATION libexec/darling/usr/lib)
