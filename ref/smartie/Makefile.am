# SPDX license identifier: MPL-2.0
# Copyright (C) 2015, Visteon Corp.
# Author: Pavel Konopelko, pkonopel@visteon.com
#
# This file is part of Common API C
#
# This Source Code Form is subject to the terms of the
# Mozilla Public License (MPL), version 2.0.
# If a copy of the MPL was not distributed with this file,
# you can obtain one at http://mozilla.org/MPL/2.0/.
# For further information see http://www.genivi.org/.

bin_PROGRAMS = smartalice smartbob

smartalice_CFLAGS = $(LIBSYSTEMD_CFLAGS) $(CAPIC_CFLAGS) -Wall -Werror
smartalice_LDFLAGS = $(LIBSYSTEMD_LIBS) $(CAPIC_LIBS)
smartalice_SOURCES = \
	src/smartalice.c \
	src-gen/server-Smartie.c \
	src-gen/server-Smartie.h \
	src-gen/client-Smartie.c \
	src-gen/client-Smartie.h

smartbob_CFLAGS = $(LIBSYSTEMD_CFLAGS) $(CAPIC_CFLAGS) -Wall -Werror
smartbob_LDFLAGS = $(LIBSYSTEMD_LIBS) $(CAPIC_LIBS)
smartbob_SOURCES = \
	src/smartbob.c \
	src-gen/server-Smartie.c \
	src-gen/server-Smartie.h \
	src-gen/client-Smartie.c \
	src-gen/client-Smartie.h
