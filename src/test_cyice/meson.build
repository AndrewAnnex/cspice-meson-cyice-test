
py.install_sources(
   [
    '__init__.py'
   ],
  pure: false,    # Will be installed next to binaries
  subdir: 'test_cyice'  # Folder relative to site-packages to install to
)

subdir('utils')

# extension module stuff for test_cyice

test_cyice = py.extension_module(
  'test_cyice',
  sources: [
    'test_cyice.pyx',
  ],
  dependencies: [
    py_dep, 
    numpy_dep,
    cspice_dep,  
    m_dep
  ],
  install: true,
)

