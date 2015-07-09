ACLOCAL_AMFLAGS = -I m4 --install
AUTOMAKE_OPTIONS = foreign

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = dmalloc.pc

LIBRARY_VERSION = 6:0:1

include_HEADERS = dmalloc.h \
	dmalloc_defs.h

lib_LTLIBRARIES = libdmalloc.la
bin_PROGRAMS = dmalloc

libdmalloc_la_SOURCES = arg_check.c \
	chunk.c \
	compat.c \
	dmalloc_rand.c \
	dmalloc_tab.c \
	env.c \
	error.c \
	heap.c \
	malloc.c

libdmalloc_la_LDFLAGS = -version-info $(LIBRARY_VERSION)

dmalloc_SOURCES = compat.c \
	dmalloc.c \
	dmalloc_argv.c \
	env.c
# define dmalloc_CFLAGS variable to ensure that
# two objects get different basenames
dmalloc_CFLAGS = $(AM_CFLAGS)
