cmake_minimum_required(VERSION 3.10)

project("Metin2 game lib")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_library(libgame STATIC 
	src/attribute.cc
	src/grid.cc
	src/targa.cc
)


target_compile_options(libgame PRIVATE -Wall -O2 -pipe -fno-exceptions)

# Pas de warning pour les caractères coréens
target_compile_options(libgame PRIVATE -Wno-invalid-source-encoding)

target_include_directories(libgame PRIVATE include)
set_target_properties(libgame PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
set_target_properties(libgame PROPERTIES OUTPUT_NAME "game")
install(TARGETS libgame LIBRARY)