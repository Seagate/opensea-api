#
# Do NOT modify or remove this copyright and license
#
# Copyright (c) 2012 - 2018 Seagate Technology LLC and/or its Affiliates, All Rights Reserved
#
# This software is subject to the terms of the Mozilla Public
# License, v. 2.0. If a copy of the MPL was not distributed with this
# file, You can obtain one at http://mozilla.org/MPL/2.0/.
#
# ******************************************************************************************

# Hand Written Makefile (Edit with caution) -Muhammad
#

CC = gcc
LIB_FILE_OUTPUT_DIR=lib
API_FILE_OUTPUT_DIR=opensea-api
STRIP = strip
CFLAGS = -Wall -c -std=gnu99
CFLAGS_I686 = -Wall -c -m32
LFLAGS = \
	-Wall \
	../../opensea-operations/Make/gcc/$(LIB_FILE_OUTPUT_DIR)/libopensea-operations.a \
	../../opensea-transport/Make/gcc/$(LIB_FILE_OUTPUT_DIR)/libopensea-transport.a \
	../../opensea-common/Make/gcc/$(LIB_FILE_OUTPUT_DIR)/libopensea-common.a
INC_DIR= \
	-I../../opensea-common/include \
	-I../../opensea-transport/include \
	-I../../include \
	-I../../opensea-operations/include

UNAME := $(shell uname)
ifeq ($(UNAME),Linux)
    LFLAGS += -lm -lrt
endif
ifeq ($(UNAME),SunOS)
    LFLAGS +=
endif
ifeq ($(UNAME),FreeBSD)
    LFLAGS += -lcam
endif

MACHINE := $(shell uname -m)

ifeq ($(MACHINE), x86_64)

endif

#add any defines needed for libraries 
#PROJECT_DEFINES += -DDISABLE_NVME_PASSTHROUGH -DDISABLE_TCG_SUPPORT
PROJECT_DEFINES += -DDISABLE_TCG_SUPPORT
ifeq ($(UNAME),FreeBSD)
PROJECT_DEFINES += -DDISABLE_NVME_PASSTHROUGH
endif

.PHONY: clean_all all

all: clean_all debug

debug: CXXFLAGS += -g
debug: CFLAGS += -g
debug: PROJECT_DEFINES += -D_DEBUG
debug: opensea-api mk_debug_package

release: CXXFLAGS += -O3
release: CFLAGS += -O3
release: opensea-api mk_release_package

static-release: LFLAGS += -static
static-release: opensea-api mk_static_package

export CFLAGS
export CXXFLAGS
export PROJECT_DEFINES

opensea-api: mkoutputdir
	$(MAKE) -C ../../opensea-common/Make/gcc
	$(MAKE) -C ../../opensea-transport/Make/gcc
	$(MAKE) -C ../../opensea-operations/Make/gcc

%.o: %.c
	$(CC) $(CFLAGS) $(INC_DIR) $(PROJECT_DEFINES) $< -o $@

clean:
	rm -f *.o *.a  -r $(API_FILE_OUTPUT_DIR)

clean_all: clean
	$(MAKE) -C ../../opensea-common/Make/gcc clean
	$(MAKE) -C ../../opensea-transport/Make/gcc clean
	$(MAKE) -C ../../opensea-operations/Make/gcc clean

copy_headers:
	cp ../../opensea-common/include/* $(API_FILE_OUTPUT_DIR)/release/headers/
	cp ../../opensea-transport/include/* $(API_FILE_OUTPUT_DIR)/release/headers/
	cp ../../opensea-operations/include/* $(API_FILE_OUTPUT_DIR)/release/headers/

mk_debug_package: copy_headers
	cp ../../opensea-common/Make/gcc/lib/* $(API_FILE_OUTPUT_DIR)/debug/
	cp ../../opensea-transport/Make/gcc/lib/* $(API_FILE_OUTPUT_DIR)/debug/
	cp ../../opensea-operations/Make/gcc/lib/* $(API_FILE_OUTPUT_DIR)/debug/

mk_release_package: copy_headers
	cp ../../opensea-common/Make/gcc/lib/* $(API_FILE_OUTPUT_DIR)/release/
	cp ../../opensea-transport/Make/gcc/lib/* $(API_FILE_OUTPUT_DIR)/release/
	cp ../../opensea-operations/Make/gcc/lib/* $(API_FILE_OUTPUT_DIR)/release/

mk_static_package: copy_headers
	cp ../../opensea-common/Make/gcc/lib/* $(API_FILE_OUTPUT_DIR)/static-release/
	cp ../../opensea-transport/Make/gcc/lib/* $(API_FILE_OUTPUT_DIR)/static-release/
	cp ../../opensea-operations/Make/gcc/lib/* $(API_FILE_OUTPUT_DIR)/static-release/

mkoutputdir:
	mkdir -p $(API_FILE_OUTPUT_DIR)/debug/headers
	mkdir -p $(API_FILE_OUTPUT_DIR)/release/headers
	mkdir -p $(API_FILE_OUTPUT_DIR)/static-release/headers
