project ('statusnotifier', 'c',
         version: '1.0.0',
         license: 'GPL3+',
         default_options:[
            'warning_level=1',
         ],
         meson_version: '>=0.59')

sni_config = configuration_data()

sni_config.set ('VERSION' , meson.project_version())
sni_config.set10 ('USE_DBUSMENU' , get_option('enable_dbusmenu'))

configure_file (
    output: 'config.h',
    configuration : sni_config
)

subdir('src')
if get_option('enable_example')
    subdir('example')
endif
