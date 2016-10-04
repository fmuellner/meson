project('vapi-test', 'c', 'vala')

gnome = import('gnome')

libfoo_sources = [
  'foo.c',
  'foo.h',
]

libfoo_deps = [
  dependency('gobject-2.0')
]

libfoo = shared_library('foo', libfoo_sources,
  dependencies: libfoo_deps,
  install: true,
)

libfoo_api_ver = '1.0'

libfoo_gir = gnome.generate_gir(libfoo,
  sources: libfoo_sources,
  namespace: 'Foo',
  nsversion: libfoo_api_ver,
  packages: 'gobject-2.0',
  symbol_prefix: 'foo',
  extra_args: [
    '--c-include=foo.h',
  ],
)

libfoo_vapi = gnome.generate_vapi('foo-' + libfoo_api_ver,
  sources: libfoo_gir[0],
  install: true,
)

vapiexe = executable('vapigen-test',
  [libfoo_vapi[0], 'main.vala'],
  dependencies: libfoo_deps,
  link_with: libfoo,
  install: true,
  vala_args: [
    '--vapidir=' + meson.current_build_dir(),
    '--pkg=foo-' + libfoo_api_ver,
    '--pkg=gobject-2.0',
  ]
)

test('vapigen-test', vapiexe)
