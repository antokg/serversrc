cmake_minimum_required(VERSION 3.10)

project("Metin2 libthecore")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_library(libthecore STATIC 
	src/des.cc
	src/fdwatch.cc
	src/gost.cc
	src/buffer.cc
	src/hangul.cc
	src/heart.cc
	src/kstbl.cc
	src/log.cc
	src/main.cc
	src/memcpy.cc
	src/signal.cc
	src/socket.cc
	src/tea.cc
	src/utils.cc
)


target_compile_options(libthecore PRIVATE -Wall -O2 -pipe)

# Pas de warning pour les caractères coréens
target_compile_options(libthecore PRIVATE -Wno-invalid-source-encoding)

target_include_directories(libthecore PRIVATE include)
set_target_properties(libthecore PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
set_target_properties(libthecore PROPERTIES OUTPUT_NAME "thecore")
install(TARGETS libthecore LIBRARY)

