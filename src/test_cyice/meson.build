
py.install_sources(
   [
    '__init__.py'
   ],
  pure: false,    # Will be installed next to binaries
  subdir: 'test_cyice'  # Folder relative to site-packages to install to
)

subdir('utils')

# for numpy
incdir_numpy = run_command(py,
  ['-c', 'import os; os.chdir(".."); import numpy; print(numpy.get_include())'],
  check : true
).stdout().strip()
inc_np = include_directories(incdir_numpy)
message('numpy inc dir: ', incdir_numpy)

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
  include_directories: [inc_np, cspice_headers],
  install: true,
  subdir: 'test_cyice'
)

