TARGET = Methane
TITLE_ID = METHANE01

CXX = arm-vita-eabi-g++
CXXFLAGS = -Wl,-q -O3 -Wall -fsigned-char -I$(VITASDK)/arm-vita-eabi/include/SDL2 -I.. -DMETHANE_OLD_CPP_NEW
LIBS = -lSDL2 -lvita2d -lmikmod -lkbdvita \
	-lSceAppMgr_stub -lSceAppUtil_stub -lSceAudio_stub -lSceCtrl_stub -lSceCommonDialog_stub \
	-lSceDisplay_stub -lSceGxm_stub -lSceHid_stub -lSceSysmodule_stub -lSceTouch_stub

# Remove the following two lines to disable sound
METHANE_SND_FLAGS = -DMETHANE_MIKMOD
METHANE_SND_LIBS =

# Methane brothers with sound
all: message $(TARGET).vpk

message:
	@echo "Compiling Super Methane Brothers."
	@echo "================================="

OBJS=\
MainSource/gfxoff.o MainSource/baddie.o MainSource/bitdraw.o MainSource/bitgroup.o MainSource/bititem.o \
	MainSource/boss.o MainSource/game.o MainSource/gasobj.o MainSource/global.o MainSource/goodie.o MainSource/maps.o \
	MainSource/misc.o MainSource/objlist.o MainSource/player.o MainSource/power.o MainSource/target.o \
	MainSource/suck.o MainSource/weapon.o MainSource/sound.o MainData/snddata.o MainData/gfxdata.o MainData/gfxdata2.o \
	MainData/gfxdata3.o MainData/mapdata.o MainMikMod/audiodrv.o MainVita/doc.o

# Vita
$(TARGET).vpk: eboot.bin
	vita-mksfoex -s TITLE_ID=$(TITLE_ID) "$(TARGET)" param.sfo
	vita-pack-vpk -s param.sfo -b eboot.bin \
	-a res/icon0.png=sce_sys/icon0.png \
	-a res/bg.png=sce_sys/livearea/contents/bg.png \
	-a res/startup.png=sce_sys/livearea/contents/startup.png \
	-a res/template.xml=sce_sys/livearea/contents/template.xml $@

eboot.bin: $(TARGET).velf
	vita-make-fself -c -s $< eboot.bin

%.velf: %.elf
	vita-elf-create $< $@

# Basic methane brothers compilation
$(TARGET).elf:  $(OBJS)
	$(CXX) $(CXXFLAGS) $(OBJS) -o $(TARGET).elf $(LIBS) $(LDFLAGS)

clean:
	@rm -Rf MainSource
	@rm -Rf MainData
	@rm -Rf MainVita
	@rm -Rf MainMikMod
	@rm -f eboot.bin param.sfo $(TARGET).elf $(TARGET).velf $(TARGET).vpk

distclean: clean


# The main source code
MainSource/%.o : ../%.cpp
	@echo "  Compiling $<..."
	@if [ ! -d MainSource ]; then mkdir -p MainSource; fi
	$(CXX) $(CXXFLAGS) $(METHANE_FLAGS) $(METHANE_SND_FLAGS) -I ../vita -c $< -o $@

# The main data
MainData/%.o : ../data/%.cpp
	@echo "  Compiling $<..."
	@if [ ! -d MainData ]; then mkdir -p MainData; fi
	$(CXX) $(CXXFLAGS) $(METHANE_FLAGS) $(METHANE_SND_FLAGS) -I ../ -c $< -o $@

# The Vita source code
MainVita/%.o : ../vita/%.cpp
	@echo "  Compiling $<..."
	@if [ ! -d MainVita ]; then mkdir -p MainVita; fi
	$(CXX) $(CXXFLAGS) $(METHANE_FLAGS) $(METHANE_SND_FLAGS) -I ../ -c $< -o $@

# The sound driver source code
MainMikMod/%.o : ../mikmod/%.cpp
	@echo "  Compiling $<..."
	@if [ ! -d MainMikMod ]; then mkdir -p MainMikMod; fi
	$(CXX) $(CXXFLAGS) $(METHANE_FLAGS) $(METHANE_SND_FLAGS) -I ../ -c $< -o $@
