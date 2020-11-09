cmake_minimum_required(VERSION 3.10)

project("Metin2 Game")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_compile_definitions(__P4_VERSION__="40250")
add_compile_definitions(__USER__="")
add_compile_definitions(__HOSTNAME__="")
add_compile_definitions(__PWD__="")
add_compile_definitions(_THREAD_SAFE)
add_compile_definitions(NDEBUG)
add_compile_definitions(__MYSQL_USE_SHA256__)

add_custom_target(limit_time python3.7 ${PROJECT_SOURCE_DIR}/src/update_limit_time.py)
add_custom_target(move cp game ${METIN2_PATH}/share/game)
add_subdirectory(../libthecore ${PROJECT_SOURCE_DIR}/../libthecore/build)
add_subdirectory(../libgame ${PROJECT_SOURCE_DIR}/../libgame/build)
add_subdirectory(../libsql ${PROJECT_SOURCE_DIR}/../libsql/build)
add_subdirectory(../libpoly ${PROJECT_SOURCE_DIR}/../libpoly/build)

add_executable(game src/BattleArena.cpp src/FSM.cpp src/MarkConvert.cpp src/MarkImage.cpp src/MarkManager.cpp src/OXEvent.cpp src/TrafficProfiler.cpp src/ani.cpp
		  src/arena.cpp src/banword.cpp src/battle.cpp src/blend_item.cpp src/block_country.cpp src/buffer_manager.cpp src/building.cpp src/castle.cpp
		  src/char.cpp src/char_affect.cpp src/char_battle.cpp src/char_change_empire.cpp src/char_horse.cpp src/char_item.cpp src/char_manager.cpp
		  src/char_quickslot.cpp src/char_resist.cpp src/char_skill.cpp src/char_state.cpp src/PetSystem.cpp src/cmd.cpp src/cmd_emotion.cpp src/cmd_general.cpp
		  src/cmd_gm.cpp src/cmd_oxevent.cpp src/config.cpp src/constants.cpp src/crc32.cpp src/cube.cpp src/db.cpp src/desc.cpp
		  src/desc_client.cpp src/desc_manager.cpp src/desc_p2p.cpp src/dev_log.cpp src/dungeon.cpp src/empire_text_convert.cpp src/entity.cpp
		  src/entity_view.cpp src/event.cpp src/event_queue.cpp src/exchange.cpp src/file_loader.cpp src/fishing.cpp src/gm.cpp src/guild.cpp
		  src/guild_manager.cpp src/guild_war.cpp src/horse_rider.cpp src/horsename_manager.cpp src/input.cpp src/input_auth.cpp src/input_db.cpp
		  src/input_login.cpp src/input_main.cpp src/input_p2p.cpp src/input_udp.cpp src/ip_ban.cpp
		  src/item.cpp src/item_addon.cpp src/item_attribute.cpp src/item_manager.cpp src/item_manager_idrange.cpp src/locale.cpp
		  src/locale_service.cpp src/log.cpp src/login_data.cpp src/lzo_manager.cpp src/marriage.cpp  src/messenger_manager.cpp
		  src/mining.cpp src/mob_manager.cpp src/monarch.cpp src/motion.cpp src/over9refine.cpp src/p2p.cpp src/packet_info.cpp
		  src/party.cpp src/polymorph.cpp src/priv_manager.cpp src/pvp.cpp src/questevent.cpp src/questlua.cpp src/questlua_affect.cpp 
		  src/questlua_arena.cpp src/questlua_ba.cpp src/questlua_building.cpp src/questlua_danceevent.cpp src/questlua_dungeon.cpp 
		  src/questlua_forked.cpp src/questlua_game.cpp src/questlua_global.cpp src/questlua_guild.cpp src/questlua_horse.cpp 
		  src/questlua_pet.cpp src/questlua_item.cpp src/questlua_marriage.cpp src/questlua_mgmt.cpp src/questlua_monarch.cpp 
		  src/questlua_npc.cpp src/questlua_oxevent.cpp src/questlua_party.cpp src/questlua_pc.cpp src/questlua_quest.cpp 
		  src/questlua_target.cpp src/questmanager.cpp src/questnpc.cpp src/questpc.cpp
		  src/refine.cpp src/regen.cpp src/safebox.cpp src/sectree.cpp src/sectree_manager.cpp src/sequence.cpp src/shop.cpp
		  src/skill.cpp src/start_position.cpp src/target.cpp src/text_file_loader.cpp src/trigger.cpp src/utils.cpp src/vector.cpp src/war_map.cpp
		  src/wedding.cpp src/xmas_event.cpp src/version.cpp src/panama.cpp src/threeway_war.cpp src/map_location.cpp
		  src/BlueDragon.cpp src/BlueDragon_Binder.cpp src/DragonLair.cpp src/questlua_dragonlair.cpp
		  src/skill_power.cpp src/affect.cpp src/SpeedServer.cpp src/questlua_speedserver.cpp 
		  src/cipher.cpp src/ClientPackageCryptInfo.cpp
		  src/buff_on_attributes.cpp src/dragon_soul_table.cpp src/DragonSoul.cpp
		  src/group_text_parse_tree.cpp src/char_dragonsoul.cpp src/questlua_dragonsoul.cpp
		  src/shop_manager.cpp src/shopEx.cpp src/item_manager_read_tables.cpp src/minilzo.c src/main.cpp
)

add_dependencies(game limit_time)
add_dependencies(game libthecore)
add_dependencies(game libgame)
add_dependencies(game libsql)
add_dependencies(game libpoly)

target_compile_options(game PRIVATE -g -Wall -O2 -pipe -fexceptions)

# Pas de warning pour les caractères coréens
target_compile_options(game PRIVATE -Wno-invalid-source-encoding)

find_library(CRYPTOPP libcryptopp.a PATHS ../../Extern/lib)
find_library(DEVIL libIL.a PATHS ../../Extern/lib)
find_library(JASPER libjasper.a PATHS ../../Extern/lib)
find_library(JPEG libjpeg.a PATHS ../../Extern/lib)
find_library(PNG libpng.a PATHS ../../Extern/lib)
find_library(TIFF libtiff.a PATHS ../../Extern/lib)
find_library(LUA liblua.a PATHS ../../Extern/lib)
find_library(LUALIB liblualib.a PATHS ../../Extern/lib)
find_library(MYSQL libmysqlclient.so /usr/local/lib/mysql)

target_include_directories(game PUBLIC ../../Extern/include)
target_link_libraries(game ${CRYPTOPP})
target_link_libraries(game ${DEVIL})
target_link_libraries(game ${JASPER})
target_link_libraries(game ${JPEG})
target_link_libraries(game ${PNG})
target_link_libraries(game ${TIFF})
target_link_libraries(game libthecore)
target_link_libraries(game libgame)
target_link_libraries(game libpoly)
target_link_libraries(game libsql)
target_link_libraries(game ${LUA})
target_link_libraries(game ${LUALIB})
target_link_libraries(game ${MYSQL})
target_link_libraries(game -pthread)
target_link_libraries(game -lmd)

# le fichier limit_time.h est généré dans le dossier de build
target_include_directories(game PUBLIC ${PROJECT_BINARY_DIR})