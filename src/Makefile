bindir ?= /usr/local/bin

AR = ar
ARFLAGS = rcs

INSTALL ?= install
INSTALL_PROGRAM ?= $(INSTALL)
INSTALL_DATA ?= $(INSTALL) -m 644

# Compiler flags:
#  -Wall turns on most but not all compiler warnings
#  -Wno-deprecated-declarations is used because CGDisplayIOServicePort was
#    deprecated by Apple with no replacement
#    https://developer.apple.com/documentation/coregraphics/1543516-cgdisplayioserviceport
WARNINGS := -Wno-deprecated-declarations
WARNINGS += -Wall
WARNINGS += -Wempty-body
WARNINGS += -Wempty-init-stmt
WARNINGS += -Wignored-qualifiers
WARNINGS += -Winitializer-overrides
WARNINGS += -Wmissing-field-initializers
WARNINGS += -Wmissing-method-return-type
WARNINGS += -Wnull-pointer-arithmetic
WARNINGS += -Woverride-init
WARNINGS += -Wredundant-move
WARNINGS += -Wsemicolon-before-method-body
WARNINGS += -Wshift-negative-value
WARNINGS += -Wstring-compare
WARNINGS += -Wtype-limits
WARNINGS += -Wuninitialized
WARNINGS += -Wunused-parameter
WARNINGS += -Wbitfield-enum-conversion
WARNINGS += -Wbool-conversion
WARNINGS += -Wconstant-conversion
WARNINGS += -Wenum-conversion
WARNINGS += -Wimplicit-float-conversion
WARNINGS += -Wimplicit-int-conversion
WARNINGS += -Wint-conversion
WARNINGS += -Wliteral-conversion
WARNINGS += -Wnon-literal-null-conversion
WARNINGS += -Wnull-conversion
WARNINGS += -Wobjc-literal-conversion
WARNINGS += -Wshorten-64-to-32
WARNINGS += -Wstring-conversion
WARNINGS += -Wshadow-field
WARNINGS += -Wshadow-field-in-constructor
WARNINGS += -Wshadow-uncaptured-local
WARNINGS += -Wformat
WARNINGS += -Wlogical-not-parentheses
WARNINGS += -Wnull-dereference
#WARNINGS += -Wextra #TODO enable after fixing these warnings

INCLUDES := -I. -F./Headers -F/System/Library/PrivateFrameworks
FRAMEWORKS := -framework IOKit -framework ApplicationServices -framework DisplayServices
FRAMEWORKS += -framework CoreDisplay -framework OSD -framework MonitorPanel -framework SkyLight

CFLAGS := $(WARNINGS)
LDFLAGS := $(INCLUDES) $(FRAMEWORKS)

OUTPUT_EXEC := displayplacer
STATIC_LIB := libDisplayPlacer.a
DYNAMIC_LIB := libDisplayPlacer.dylib

.PHONY: all clean debug install
all: $(OUTPUT_EXEC)

$(OUTPUT_EXEC):
	$(CC) $(CFLAGS) $(CPPFLAGS) $(LDFLAGS) DisplayPlacer.c Legacy/v130.c -x objective-c MonitorPanel.m -o $(OUTPUT_EXEC)

$(STATIC_LIB): DisplayPlacer.o
	$(AR) rcs $(STATIC_LIB) DisplayPlacer.o

$(DYNAMIC_LIB): DisplayPlacer.o
	$(CC) $(LDFLAGS) -dynamiclib -o $(DYNAMIC_LIB) DisplayPlacer.o MonitorPanel.o v130.o

DisplayPlacer.o:
	$(CC) $(CFLAGS) -c DisplayPlacer.c Legacy/v130.c -x objective-c MonitorPanel.m $(INCLUDES)

debug: CFLAGS += -g
debug: $(OUTPUT_EXEC)

install: $(OUTPUT_EXEC)
	$(INSTALL_PROGRAM) $(OUTPUT_EXEC) $(DESTDIR)$(bindir)

clean:
	rm -f $(OUTPUT_EXEC) *.o $(STATIC_LIB) $(DYNAMIC_LIB)