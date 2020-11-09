cmake_minimum_required(VERSION 3.10)

project("Metin2 libpoly")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_library(libpoly STATIC 
	src/Base.cc
	src/Poly.cc
	src/SymTable.cc
	src/Symbol.cc
)


target_compile_options(libpoly PRIVATE -Wall -O2 -pipe -fno-exceptions -fno-rtti)

# Pas de warning pour les caractères coréens
target_compile_options(libpoly PRIVATE -Wno-invalid-source-encoding)

target_include_directories(libpoly PRIVATE include)
set_target_properties(libpoly PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
set_target_properties(libpoly PROPERTIES OUTPUT_NAME "poly")
install(TARGETS libpoly LIBRARY)