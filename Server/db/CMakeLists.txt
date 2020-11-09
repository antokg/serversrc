cmake_minimum_required(VERSION 3.10)

project("Metin2 db")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_compile_definitions(__P4_VERSION__="40250")
add_compile_definitions(__USER__="")
add_compile_definitions(__HOSTNAME__="")
add_compile_definitions(__PWD__="")
add_compile_definitions(_THREAD_SAFE)

add_custom_target(move cp db ${METIN2_PATH}/db/db)

add_subdirectory(../libthecore ${PROJECT_SOURCE_DIR}/../libthecore/build)
add_subdirectory(../libgame ${PROJECT_SOURCE_DIR}/../libgame/build)
add_subdirectory(../libsql ${PROJECT_SOURCE_DIR}/../libsql/build)
add_subdirectory(../libpoly ${PROJECT_SOURCE_DIR}/../libpoly/build)

add_executable(db 
	src/BlockCountry.cpp
	src/Cache.cpp
	src/ClientManager.cpp
	src/ClientManagerBoot.cpp
	src/ClientManagerEventFlag.cpp
	src/ClientManagerGuild.cpp
	src/ClientManagerHorseName.cpp
	src/ClientManagerLogin.cpp
	src/ClientManagerParty.cpp
	src/ClientManagerPlayer.cpp
	src/Config.cpp
	src/CsvReader.cpp
	src/DBManager.cpp
	src/GuildManager.cpp
	src/HB.cpp
	src/ItemAwardManager.cpp
	src/ItemIDRangeManager.cpp
	src/Lock.cpp
	src/LoginData.cpp
	src/Main.cpp
	src/Marriage.cpp
	src/Monarch.cpp
	src/MoneyLog.cpp
	src/NetBase.cpp
	src/Peer.cpp
	src/PeerBase.cpp
	src/PrivManager.cpp
	src/ProtoReader.cpp
	src/version.cpp
)

add_dependencies(db libthecore)
add_dependencies(db libgame)
add_dependencies(db libsql)
add_dependencies(db libpoly)

target_compile_options(db PRIVATE -g -Wall -O2 -pipe -fno-rtti -fno-exceptions)

# Pas de warning pour les caractères coréens
target_compile_options(db PRIVATE -Wno-invalid-source-encoding)

find_library(MYSQL libmysqlclient.so /usr/local/lib/mysql)

target_include_directories(db PUBLIC ../../Extern/include)
target_link_libraries(db libthecore)
target_link_libraries(db libgame)
target_link_libraries(db libpoly)
target_link_libraries(db libsql)
target_link_libraries(db ${MYSQL})
target_link_libraries(db -pthread)
