cmake_minimum_required(VERSION 3.10)

project("Metin2 libsql")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_compile_definitions(_THREAD_SAFE)

add_library(libsql STATIC 
	src/AsyncSQL.cpp
	src/Semaphore.cpp
	src/Statement.cpp
	src/Tellwait.cpp
)


target_compile_options(libsql PRIVATE -Wall -O2 -pipe -fno-exceptions )

# Pas de warning pour les caractères coréens
target_compile_options(libsql PRIVATE -Wno-invalid-source-encoding)

target_include_directories(libsql PRIVATE ../../Extern/include)
target_include_directories(libsql PRIVATE include)
set_target_properties(libsql PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
set_target_properties(libsql PROPERTIES OUTPUT_NAME "sql")
install(TARGETS libsql LIBRARY)