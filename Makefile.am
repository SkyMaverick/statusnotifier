
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
    src/statusnotifier.c \
    src/interfaces.h

EXTRA_DIST = \
    src/closures \
    $(TEMPL_CLOSURES) \
    $(TEMPL_ENUMS_C)\
    $(TEMPL_ENUMS_H)\
    src/mkenums \
    m4/introspection.m4

if HAVE_INTROSPECTION
BUILT_GIRSOURCES = StatusNotifier-$(GIR_VERSION).gir

GIR_EXTRA =
if USE_DBUSMENU
GIR_EXTRA += -DUSE_DBUSMENU=1 --include=Gtk-3.0
endif
StatusNotifier-$(GIR_VERSION).gir: $(INTROSPECTION_SCANNER) libstatusnotifier.la
	$(AM_V_GEN) $(INTROSPECTION_SCANNER) \
		-v --warn-all -n StatusNotifier --nsversion=$(GIR_VERSION) \
		--include=GObject-2.0 --include=GdkPixbuf-2.0 \
		$(INCLUDES) $(GIR_EXTRA) \
		--library=statusnotifier -o $@ \
		src/statusnotifier.[ch] src/enums.[ch]

girdir = $(datadir)/gir-1.0
gir_DATA = $(BUILT_GIRSOURCES)

typelibsdir = $(libdir)/girepository-1.0
typelibs_DATA = $(BUILT_GIRSOURCES:.gir=.typelib)

%.typelib: %.gir $(INTROSPECTION_COMPILER)
	$(AM_V_GEN) $(INTROSPECTION_COMPILER) \
		--includedir=$(srcdir) --includedir=. $(INTROSPECTION_COMPILER_OPTS) \
		$< -o $(@F)

CLEANFILES += $(BUILT_GIRSOURCES) $(typelibs_DATA) $(GLIB_GENERATED_FILES)
endif
