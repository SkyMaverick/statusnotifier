
CLEANFILES =

SUBDIRS = . docs/reference
if EXAMPLE
SUBDIRS += example
endif

ACLOCAL_AMFLAGS = -I m4

if USE_GIT_VERSION
_VERSION = `git describe --abbrev=4 --dirty`
DEFS += -DGIT_VERSION=\"$(_VERSION)\"
else
_VERSION = $(PACKAGE_VERSION)
endif

AM_CFLAGS = \
	-g \
	-DDOCDIR='"$(docdir)"' \
	${WARNING_CFLAGS}

lib_LTLIBRARIES = libstatusnotifier.la
include_HEADERS = src/statusnotifier.h src/statusnotifier-compat.h

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = statusnotifier.pc

TEMPL_CLOSURES = src/closures.template
TEMPL_ENUMS_H = src/enums.h.template
TEMPL_ENUMS_C = src/enums.c.template

src/closures.h: $(TEMPL_CLOSURES)
		$(AM_V_GEN)$(GLIB_GENMARSHAL) --header --output=$@ $<

src/closures.c: $(TEMPL_CLOSURES) src/closures.h
		$(AM_V_GEN)$(GLIB_GENMARSHAL) --include-header=src/closures.h --body --output=$@ $<

src/enums.h: $(include_HEADERS) $(TEMPL_ENUMS_H)
		$(AM_V_GEN)$(GLIB_MKENUMS) --template=$(TEMPL_ENUMS_H) --output=$@ $(include_HEADERS)

src/enums.c: $(include_HEADERS) $(TEMPL_ENUMS_C) src/enums.h
		$(AM_V_GEN)$(GLIB_MKENUMS) --template=$(TEMPL_ENUMS_C) --output=$@ $(include_HEADERS)

GLIB_GENERATED_FILES = \
	src/closures.h \
	src/closures.c \
	src/enums.h \
	src/enums.c

libstatusnotifier_la_LDFLAGS = -version-info $(LIB_VERSION_INFO)
libstatusnotifier_la_CFLAGS = ${AM_CFLAGS} @DEP_CFLAGS@
libstatusnotifier_la_LIBADD = @DEP_LIBS@
libstatusnotifier_la_SOURCES = \
    $(GLIB_GENERATED_FILES) \
    src/statusnotifier.h \
    src/statusnotifier.c

EXTRA_DIST = \
    src/closures \
    $(TEMPL_CLOSURES) \
    $(TEMPL_ENUMS_C)\
    $(TEMPL_ENUMS_H)\
    src/mkenums \
    src/interfaces.h \
    m4/introspection.m4

-include $(INTROSPECTION_MAKEFILE)
INTROSPECTION_GIRS =
INTROSPECTION_SCANNER_ARGS = --add-include-path=$(srcdir)/src --warn-all
INTROSPECTION_COMPILER_ARGS = --includedir=$(srcdir)/src

if HAVE_INTROSPECTION
introspection_sources = \
	src/enums.h \
	src/enums.c \
    src/statusnotifier.h \
    src/statusnotifier.c

StatusNotifier-1.0.gir: $(lib_LTLIBRARIES)
StatusNotifier_1_0_gir_INCLUDES = GObject-2.0 GdkPixbuf-2.0
if USE_DBUSMENU
StatusNotifier_1_0_gir_INCLUDES += Dbusmenu-0.4
endif
StatusNotifier_1_0_gir_CFLAGS = $(AM_CFLAGS)
StatusNotifier_1_0_gir_LIBS = $(lib_LTLIBRARIES)
StatusNotifier_1_0_gir_FILES = $(introspection_sources)
StatusNotifier_1_0_gir_VERSION = $(GIR_VERSION)

INTROSPECTION_GIRS += StatusNotifier-1.0.gir

girdir = $(datadir)/gir-1.0
gir_DATA = $(INTROSPECTION_GIRS)

typelibdir = $(libdir)/girepository-1.0
typelib_DATA = $(INTROSPECTION_GIRS:.gir=.typelib)

CLEANFILES += $(gir_DATA) $(typelib_DATA)
endif

CLEANFILES += $(GLIB_GENERATED_FILES)
