example_deps = [
    dependency ('gtk+-3.0', version: '>=3.18')
]
example_sni = files('''
        sn-example.c
'''.strip())

sni_lib = executable ('sn-example', example_sni,
        dependencies: example_deps,
        include_directories: sni_incs,
        link_with: sni_lib,
        install: false)
