# DTLS Examples Makefile
CC       = gcc
WOLFSSL_INSTALL_DIR ?= /usr/local
CFLAGS   = -Wall -I$(WOLFSSL_INSTALL_DIR)/include
LIBS     = -L$(WOLFSSL_INSTALL_DIR)/lib -lm
UNAME_S  := $(shell uname -s)
WIN_LIBS :=

ifeq ($(OS),Windows_NT)
WOLFSSL_INSTALL_DIR = /ucrt64
WIN_LIBS += -lws2_32
endif

ifneq (,$(findstring MINGW,$(UNAME_S)))
WOLFSSL_INSTALL_DIR = /ucrt64
WIN_LIBS += -lws2_32
endif

LIBS += $(sort $(WIN_LIBS))

# option variables
DYN_LIB         = -lwolfssl
STATIC_LIB      = $(WOLFSSL_INSTALL_DIR)/lib/libwolfssl.a
DEBUG_FLAGS     = -g3 -DDEBUG -O0
DEBUG_INC_PATHS = -MD
OPTIMIZE        = -Os

# Options
#CFLAGS+=$(DEBUG_FLAGS)
CFLAGS+=$(OPTIMIZE)
#LIBS+=$(STATIC_LIB)
LIBS+=$(DYN_LIB)

# build targets
SRC=$(wildcard *.c)
TARGETS=$(patsubst %.c, %, $(SRC))

.PHONY: clean all

all: $(TARGETS)

debug: CFLAGS+=$(DEBUG_FLAGS)
debug: all

# add the -pthread flag and -lpthread lib to any threaded or shared examples
%-threaded: CFLAGS+=-pthread
%-threaded: LIBS+=-lpthread
%-shared: CFLAGS+=-pthread
%-shared: LIBS+=-lpthread

# try to build the libevent server
server-dtls13-event: server-dtls13-event.c
	$(CC) -o $@ $< $(CFLAGS) $(LIBS) -levent

# build template
%: %.c
	$(CC) -o $@ $< $(CFLAGS) $(LIBS)

clean:
	rm -f $(TARGETS)
