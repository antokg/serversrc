PLATFORM = $(shell file /bin/ls | cut -d' ' -f3 | cut -d'-' -f1)

# GCC_VERSION = $(shell $(CXX) --version 2>&1 | grep "(GCC)" | cut -d' ' -f3  | cut -d'.' -f1)
# BSD_VERSION = $(shell uname -v 2>&1 | cut -d' ' -f2 | cut -d'.' -f1)
MYSQL_VERSION = $(shell mysql --version | cut -d" " -f4 | cut -d"." -f1)
P4_VERSION = 40250

C = clang
CXX = clang++

INCDIR =
LIBDIR =
BINDIR = ..
OBJDIR = OBJDIR
$(shell if [ ! -d $(OBJDIR) ]; then mkdir $(OBJDIR); fi)

# Standard Setting
LIBS = -pthread -lm -lmd
CXXFLAGS = -std=c++17
CFLAGS = -g -Wall -O2 -pipe -fexceptions -D_THREAD_SAFE -DNDEBUG -Wno-invalid-source-encoding

# pour les versions 5.x.x le numero de version est 14
ifneq ($(MYSQL_VERSION), 14)
CFLAGS += -D__MYSQL_USE_SHA1__
endif

# boost
INCDIR += -I../../../Extern/include/boost

# Miscellaneous external libraries
INCDIR += -I../../../Extern/include
LIBDIR += -L../../../Extern/lib



# MySQL
INCDIR += -I../../libmysql/7.x-5.1.35
LIBDIR += -L../../libmysql/7.x-5.1.35

LIBS += /usr/local/lib/mysql/libmysqlclient.so -lz

LIBS += -lcryptopp
LIBS += -lpng -ljasper -ljpeg -ltiff -llzma -lIL

# Project Library
INCDIR += -I../../liblua/include
INCDIR += -I/usr/local/include
# INCDIR += -L/usr/local/lib/mysql
LIBDIR += -L../../libthecore/lib -L../../libsql -L../../libgame/lib -L../../liblua/lib
LIBDIR += -L/usr/local/lib
LIBS += -lthecore -llua -llualib -lsql -lgame
LIBS += ../../libpoly/libpoly.a

TARGET  = $(BINDIR)/game_r$(P4_VERSION)_$(PLATFORM)
OUTPUT = /home/metin2/share/game

CFILE	= minilzo.c

CPPFILE = BattleArena.cpp FSM.cpp MarkConvert.cpp MarkImage.cpp MarkManager.cpp OXEvent.cpp TrafficProfiler.cpp ani.cpp\
		  arena.cpp banword.cpp battle.cpp blend_item.cpp block_country.cpp buffer_manager.cpp building.cpp castle.cpp\
		  char.cpp char_affect.cpp char_battle.cpp char_change_empire.cpp char_horse.cpp char_item.cpp char_manager.cpp\
		  char_quickslot.cpp char_resist.cpp char_skill.cpp char_state.cpp PetSystem.cpp cmd.cpp cmd_emotion.cpp cmd_general.cpp\
		  cmd_gm.cpp cmd_oxevent.cpp config.cpp constants.cpp crc32.cpp cube.cpp db.cpp desc.cpp\
		  desc_client.cpp desc_manager.cpp desc_p2p.cpp dev_log.cpp dungeon.cpp empire_text_convert.cpp entity.cpp\
		  entity_view.cpp event.cpp event_queue.cpp exchange.cpp file_loader.cpp fishing.cpp gm.cpp guild.cpp\
		  guild_manager.cpp guild_war.cpp horse_rider.cpp horsename_manager.cpp input.cpp input_auth.cpp input_db.cpp\
		  input_login.cpp input_main.cpp input_p2p.cpp input_udp.cpp ip_ban.cpp\
		  item.cpp item_addon.cpp item_attribute.cpp item_manager.cpp item_manager_idrange.cpp locale.cpp\
		  locale_service.cpp log.cpp login_data.cpp lzo_manager.cpp marriage.cpp  messenger_manager.cpp mining.cpp mob_manager.cpp monarch.cpp motion.cpp over9refine.cpp p2p.cpp packet_info.cpp\
		  party.cpp polymorph.cpp priv_manager.cpp pvp.cpp questevent.cpp questlua.cpp questlua_affect.cpp questlua_arena.cpp questlua_ba.cpp questlua_building.cpp\
		  questlua_danceevent.cpp questlua_dungeon.cpp questlua_forked.cpp questlua_game.cpp questlua_global.cpp\
		  questlua_guild.cpp questlua_horse.cpp questlua_pet.cpp questlua_item.cpp questlua_marriage.cpp questlua_mgmt.cpp\
		  questlua_monarch.cpp questlua_npc.cpp questlua_oxevent.cpp questlua_party.cpp questlua_pc.cpp\
		  questlua_quest.cpp questlua_target.cpp questmanager.cpp questnpc.cpp questpc.cpp\
		  refine.cpp regen.cpp safebox.cpp sectree.cpp sectree_manager.cpp sequence.cpp shop.cpp\
		  skill.cpp start_position.cpp target.cpp text_file_loader.cpp trigger.cpp utils.cpp vector.cpp war_map.cpp\
		  wedding.cpp xmas_event.cpp version.cpp panama.cpp threeway_war.cpp map_location.cpp\
		  BlueDragon.cpp BlueDragon_Binder.cpp DragonLair.cpp questlua_dragonlair.cpp\
		  skill_power.cpp affect.cpp SpeedServer.cpp questlua_speedserver.cpp \
		  cipher.cpp ClientPackageCryptInfo.cpp\
		  buff_on_attributes.cpp dragon_soul_table.cpp DragonSoul.cpp\
		  group_text_parse_tree.cpp char_dragonsoul.cpp questlua_dragonsoul.cpp\
		  shop_manager.cpp shopEx.cpp item_manager_read_tables.cpp


COBJS	= $(CFILE:%.c=$(OBJDIR)/%.o)
CPPOBJS	= $(CPPFILE:%.cpp=$(OBJDIR)/%.o)

MAINOBJ = $(OBJDIR)/main.o
MAINCPP = main.cpp

default: $(TARGET)

$(OBJDIR)/minilzo.o: minilzo.c
	@$(C) $(CFLAGS) $(INCDIR) -c $< -o $@
	@echo compile $<

$(OBJDIR)/version.o: version.cpp
	@$(CXX) $(CXXFLAGS) $(CFLAGS) -D__USER__=\"$(USER)\" -D__HOSTNAME__=\"$(HOSTNAME)\" -D__PWD__=\"$(PWD)\" -D__P4_VERSION__=\"$(P4_VERSION)\" -c $< -o $@
	@echo compile $<

$(OBJDIR)/%.o: %.cpp
	@echo compile $<
	@$(CXX) $(CXXFLAGS) $(CFLAGS) $(INCDIR) -c $< -o $@

limit_time:
	@echo update limit time
	@python3.7 update_limit_time.py

$(TARGET): $(CPPOBJS) $(COBJS) $(MAINOBJ)
	@echo linking $(TARGET)....
	@$(CXX) $(CXXFLAGS) $(CFLAGS) $(LIBDIR) $(COBJS) $(CPPOBJS) $(MAINOBJ) $(LIBS) -o $(TARGET)

clean:
	@rm -f $(COBJS) $(CPPOBJS) $(MAINOBJ)
	@rm -f $(BINDIR)/game_r* $(BINDIR)/conv

tag:
	ctags *.cpp *.h *.c

dep:
	makedepend -f Depend $(INCDIR) -p$(OBJDIR)/ $(CPPFILE) $(CFILE) $(MAINCPP) 2> /dev/null > Depend
	
install:
	@cp $(TARGET) $(OUTPUT)

sinclude Depend
