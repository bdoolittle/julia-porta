# LAST EDIT: Fri Sep 20 15:09:27 2002 by Andreas Loebel (opt0.zib.de)
# $Id: Makefile,v 1.3 2002/09/20 13:09:40 bzfloebe Exp $

CFLAGS = -O6 -g -Wall
CC     ?= gcc
VPATH  = ../src

LIB_MOD    = common arith inout log
valid_MOD  = valid $(LIB_MOD)
xporta_MOD = porta four_mot portsort largecalc mp $(LIB_MOD)
ALL_MOD    = $(valid_MOD) $(xporta_MOD)

# `exeext` is the extension, including the dot, of executables.
# These variables are preset in the BinaryBuilder.jl environment.
UNAME := $(shell uname)
ifeq ($(UNAME), Darwin)
	exeext ?=
else ifeq ($(UNAME), Windows)
	exeext ?= .exe
else
	exeext ?=
endif

VALID_EXE := valid$(exeext)
XPORTA_EXE := xporta$(exeext)

all: $(XPORTA_EXE) $(VALID_EXE)

$(VALID_EXE): $(valid_MOD:%=%.d) $(valid_MOD:%=%.o)
	$(CC) -o bin/$@ $(valid_MOD:%=%.o)

$(XPORTA_EXE): $(xporta_MOD:%=%.d) $(xporta_MOD:%=%.o)
	$(CC) -o bin/$@ $(xporta_MOD:%=%.o)


clean:
	rm -f *.o *.d bin/xporta bin/valid

%.d: %.c
	$(SHELL) -ec '$(CC) -MM $(CPPFLAGS) $< \
		| sed '\''s/\($*\)\.o[ :]*/\1.o $@ : /g'\'' > $@; \
		[ -s $@ ] || rm -f $@'

include $(ALL_MOD:%=%.d)
