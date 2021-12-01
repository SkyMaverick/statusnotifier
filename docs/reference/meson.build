doc_verxml = configure_file (
            input: files('version.xml.in'),
            output: 'version.xml',
            configuration: {
                'PACKAGE_VERSION' : meson.project_version()
            })

ignore_files = '''
    statusnotifier-compat.h
    interfaces.h
    config.h
'''.split()

sni_gtk_doc = gnome_rt.gtkdoc ('gtkdoc',
            content_files: doc_verxml ,
            dependencies: [sni_lib, sni_deps] ,
            ignore_headers: ignore_files,
            main_sgml: meson.project_name() + '-docs.xml',
            mkdb_args: ['--xml-mode','--output-format=xml'] ,
            namespace: 'status_notifier_item' ,
            module_version: meson.project_version(),
            src_dir: [meson.global_source_root()/'src', sni_source_build_dir],
            install: true)
