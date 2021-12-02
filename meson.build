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

pkgconf_app = import ('pkgconfig')
pkgconf_app.generate(
    libraries: [sni_lib],
      version: meson.project_version(),
         name: 'StatusNotifier',
     filebase: meson.project_name(),
  description: 'Status Notifier Item Library',
     requires: sni_deps_array,
  install_dir: get_option('datadir')/'pkgconfig')

# Generate example app
if get_option('enable_example')
    subdir('example')
endif
# Generate documentation
if get_option('enable_docs')
    subdir('docs/reference')
endif
