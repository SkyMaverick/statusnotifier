project ('statusnotifier', 'c',
         version: '1.0.0',
         license: 'GPL3+',
         default_options:[
            'warning_level=2',

         ],
         meson_version: '>=0.59')

# Dependency-packages

#FIXME Actual dependencies versions
sni_deps_list = [
    ['gobject-2.0',     '>=2.0'],
    ['gio-2.0',         '>=2.0'],
    ['gdk-pixbuf-2.0',  '>=2.0'],
    ['gdk-3.0',         '>=3.0'],
]
if get_option('enable_dbusmenu')
    sni_deps_list += [
        ['dbusmenu-glib-0.4', '>=16.04'],
        ['dbusmenu-gtk3-0.4', '>=16.04']
    ]
endif

# Build library

subdir('src')

# Generate pkg-config file

sni_deps_array = []
foreach dep : sni_deps_list
    sni_deps_array += dep[0]
endforeach

sni_pkg_conf = configure_file (
    input: files('statusnotifier.pc.in'),
    output: 'statusnotifier.pc',
    configuration: {
        'prefix'        : get_option('prefix'),
        'exec_prefix'   : get_option('prefix'), #FIXME Meson don't define exec_prefix,
        'libdir'        : '${prefix}'/get_option('libdir'),
        'includedir'    : '${prefix}'/get_option('includedir'),

        'VERSION' : meson.project_version(),
        'DEP_PACKAGES' : ' '.join(sni_deps_array)
    },
    install: true,
    install_dir: get_option('datadir')/'pkgconfig')

# Generate example app
if get_option('enable_example')
    subdir('example')
endif
# Generate documentation
if get_option('enable_docs')
    subdir('docs/reference')
endif
