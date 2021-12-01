sni_source = files ('''
    statusnotifier.c
'''.split())

sni_source_h = files ('''
    statusnotifier.h
'''.split())

gnome_rt = import('gnome')
sni_source += gnome_rt.mkenums ('gen-enums',
            sources: sni_source_h,
            h_template: 'enums.h.template',
            c_template: 'enums.c.template',
            install_header: false)

sni_source += gnome_rt.genmarshal('closures',
            sources: 'closures.template')

sni_incs = include_directories('''
        .
'''.split())

sni_deps = [
    dependency('gobject-2.0'),
    dependency('gio-2.0'),
    dependency('gdk-3.0'),
    dependency('gdk-pixbuf-2.0')
]
if get_option('enable_dbusmenu')
    sni_deps += [
        dependency('dbusmenu-glib-0.4'),
        dependency('dbusmenu-gtk3-0.4'),
    ]
endif

sni_config = configuration_data()

sni_config.set ('VERSION' , meson.project_version())
sni_config.set10 ('USE_DBUSMENU' , get_option('enable_dbusmenu'))

configure_file (
    output: 'config.h',
    configuration : sni_config)

sni_lib = shared_library ('sni', sni_source,
        dependencies: sni_deps,
        install: false)

# Generate GIR and bindings for external dependencies
if get_option('enable_introspection')
    sni_gir_incs = '''
        GObject-2.0
        GdkPixbuf-2.0
        Dbusmenu-0.4
    '''.split()

    sni_gir_extra = ''' 
        --no-libtool 
    '''.split()

    sni_gir = gnome_rt.generate_gir (
                   sni_lib,
                   sources: sni_source + sni_source_h,
                   namespace: 'status_notifier',
                   nsversion: meson.project_version(),
                   identifier_prefix: 'StatusNotifier',
                   symbol_prefix: 'status_notifier',
                   export_packages: meson.project_name(),
                   includes: sni_gir_incs,
                   header: 'statusnotifier.h',
                   extra_args: sni_gir_extra)

    sni_extern = declare_dependency (
                include_directories: sni_incs,
                link_with: sni_lib)

    sni_vapi_deps = '''
        gobject-2.0
        gtk+-3.0
        Dbusmenu-0.4
    '''.split()

    sni_vapi = gnome_rt.generate_vapi (
            meson.project_name(),
            sources: sni_gir[0],
            packages: sni_vapi_deps)

    sni_extern_vala = declare_dependency (
                include_directories: sni_incs,
                link_with: sni_lib,
                dependencies: sni_vapi)
endif
