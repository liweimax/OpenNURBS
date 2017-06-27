# OpenNURBS CMake Build system
# see GNU GENERAL PUBLIC LICENSE, version 2, attached


set(name OpenNURBS_SDK)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")

include_directories(
	.
	inc
	zlib
	${CMAKE_BINARY_DIR}/sdk/zlib
)

link_directories(
	${CMAKE_SOURCE_DIR}/bin
)

set(libname OpenNURBS)
FILE(GLOB libsources 
  src/*.cpp
  src/*.c
  inc/*.h
)
add_definitions(-DUNICODE -DON_COMPILING_OPENNURBS -DON_DLL_EXPORTS)
add_library(${libname} ${libsources})
set_target_properties(${libname} PROPERTIES LINKER_LANGUAGE CXX)


target_link_libraries(${libname} debug zlibstaticd optimized zlibstatic)
add_dependencies(${libname} zlibstatic)

if(MSVC)
target_link_libraries(${libname} opengl32 glu32 Rpcrt4)
endif(MSVC)

set(LIBRARY_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/bin)

install(TARGETS OpenNURBS DESTINATION bin)


