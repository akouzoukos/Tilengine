# CC       = gcc
SOURCES  = $(filter-out Test.c,$(wildcard *.c))
OBJECTS  = $(SOURCES:.c=.o)
INCPATH  = ../include

# Windows specific flags
ifeq ($(OS),Windows_NT)
$(error This Makefile is not intended for Windows! Please use Visual Studio instead)
else
	RM = rm -f
	name := $(shell uname -s)
	arch := $(shell uname -m)
# Linux specific flags (i686, x64 and arm)
	ifeq ($(name),Linux)
		LIBS = -lc
		BIN  = libTilengine.a
		LIBPATH = ../lib/linux_$(arch)
	endif
# OSX specific flags
	ifeq ($(name),Darwin)
		LIBS = -lpng -lz -framework SDL2 -lc
		BIN  = Tilengine.dylib
		LDFLAGS = -dynamiclib
		LIBPATH = ../lib/darwin_x86_64
	endif
endif

# common flags
CFLAGS += -I$(INCPATH) -std=c99 -O2 -DLIB_EXPORTS
CFLAGS += -Wno-unused-result -Wno-format-truncation

.PHONY: all all-before all-after clean clean-custom

all: all-before $(BIN) all-after
	echo $(name)
	echo $(arch)

clean: clean-custom
	${RM} $(OBJECTS) $(BIN) Test

$(BIN): $(OBJECTS)
	$(AR) rcs $(BIN) $(OBJECTS) $(LIBS)
	mkdir -p $(LIBPATH)
	cp $(BIN) $(LIBPATH)

Test: Test.c
	$(RM) Test.o Test
	$(CC) Test.c -o Test -I$(INCPATH) -L$(LIBPATH) -lTilengine -Wl,-rpath,$(LIBPATH)

%.o: %.c
	$(CC) -c $(CFLAGS) $< -o $@
