sni_source = files ('''
    statusnotifier.c
'''.split())

sni_source_h = files ('''
    statusnotifier.h
'''.split())
install_headers(sni_source_h)

sni_incs = include_directories('''
        .
'''.split())

# Generate enums[c,h]

gnome_rt = import('gnome')
sni_enums = gnome_rt.mkenums ('enums',
            sources: sni_source_h,
            h_template: 'enums.h.template',
            c_template: 'enums.c.template',
            install_header: false)
sni_source += sni_enums
sni_source += gnome_rt.genmarshal('closures',
            sources: 'closures.template')

sni_deps = []
foreach dep : sni_deps_list
    sni_deps += dependency(dep[0], version: dep[1])
endforeach

# Configuration file

sni_config = configuration_data()

sni_config.set   ('VERSION' ,       meson.project_version())
sni_config.set10 ('USE_DBUSMENU' ,  get_option('enable_dbusmenu'))

configure_file (
    output: 'config.h',
    configuration : sni_config)

sni_lib = shared_library (meson.project_name(), sni_source,
        dependencies: sni_deps,
        install: true)

sni_extern = declare_dependency (
            include_directories: sni_incs,
            link_with: sni_lib)

sni_source_build_dir = meson.current_build_dir()

# Generate GIR and bindings for external dependencies

if get_option('enable_introspection')
    sni_gir_incs = '''
        GObject-2.0
        GdkPixbuf-2.0
    '''.split()

    sni_vapi_deps = '''
        gobject-2.0
        gtk+-3.0
    '''.split()

    if get_option('enable_dbusmenu')
        sni_gir_incs  += 'Dbusmenu-0.4'
        sni_vapi_deps += 'Dbusmenu-0.4'
    endif

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
                   extra_args: sni_gir_extra,
                   install: true,
                   install_dir_gir: get_option('datadir')/'gir-1.0',
                   install_dir_typelib: get_option('libdir')/'girepository-1.0')

    if get_option('enable_vala')
        sni_vapi = gnome_rt.generate_vapi (
                meson.project_name(),
                sources: sni_gir[0],
                packages: sni_vapi_deps,
                install: true)

        sni_extern_vala = declare_dependency (
                    include_directories: sni_incs,
                    link_with: sni_lib,
                    dependencies: sni_vapi)
    endif
endif
