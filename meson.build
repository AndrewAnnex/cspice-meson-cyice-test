project(
    'test_cyice', 
    'c', 
    'cython',
    default_options : ['warning_level=3', 'buildtype=release']
    )

fs = import('fs')

# get basic info
host = host_machine
system = host.system()
arch = host.cpu_family()
is_linux   = system == 'linux'
is_windows = system == 'windows'
is_macos   = system == 'darwin'
is_cygwin  = system == 'cygwin'
is_freebsd = system == 'freebsd'
is_arm     = arch == 'aarch64'
is_x86_64  = arch == 'x86_64'

# Determine which fallback dependency to use based on platform
cspice_wrap_name = ''
if is_macos and is_arm
  cspice_wrap_name = 'libcspice-macos-arm'
elif is_macos and is_x86_64
  cspice_wrap_name = 'libcspice-macos-x64'
elif is_linux and is_arm
  cspice_wrap_name = 'libcspice-linux-arm'
elif is_linux and is_x86_64
  cspice_wrap_name = 'libcspice-linux-x64'
elif is_freebsd
  cspice_wrap_name = 'libcspice-linux-x64'
elif is_cygwin
  cspice_wrap_name = 'libcspice-cygwin-x64'
elif is_windows
  cspice_wrap_name = 'libcspice-windows-x64'
else
  error('Unsupported system or architecture: @0@/@1@'.format(system, arch))
endif
# allow potential override
override = get_option('cspice_wrap')
if override != ''
  cspice_wrap_name = override
endif

cspice_dep = dependency(
    'libcspice', 
    fallback: cspice_wrap_name,
)

# grab C compiler object
cc = meson.get_compiler('c')

m_dep = cc.find_library('m', required : false)
if m_dep.found()
  add_project_link_arguments('-lm', language : 'c')
endif

if host_machine.system() == 'darwin'
  if cc.has_link_argument('-Wl,-dead_strip')
    # Allow linker to strip unused symbols
    add_project_link_arguments('-Wl,-dead_strip', language : ['c', 'cpp', 'fortran'])
  endif
endif

py3 = import('python').find_installation(pure: false)
py3_dep = py3.dependency()

_numpy_dep = dependency('numpy')

incdir_numpy = meson.get_external_property('numpy-include-dir', 'not-given')
if incdir_numpy == 'not-given'
  incdir_numpy = run_command(py3,
    [
      '-c',
      '''import os
import numpy as np
incdir = os.path.relpath(np.get_include())
print(incdir)
  '''
    ],
    check: true
  ).stdout().strip()

  # We do need an absolute path to feed to `cc.find_library` below
  _incdir_numpy_abs = run_command(py3,
    ['-c', 'import os; os.chdir(".."); import numpy; print(numpy.get_include())'],
    check: true
  ).stdout().strip()
else
  _incdir_numpy_abs = incdir_numpy
endif
message('Numpy include dir: ' + _incdir_numpy_abs)

inc_np = include_directories(_incdir_numpy_abs)
# Don't use the deprecated NumPy C API. Define this to a fixed version instead of
# NPY_API_VERSION in order not to break compilation for released SciPy versions
# when NumPy introduces a new deprecation.
numpy_nodepr_api = ['-DNPY_NO_DEPRECATED_API=NPY_1_9_API_VERSION']
np_dep = declare_dependency(include_directories: inc_np, compile_args: numpy_nodepr_api)


test_cyice = py3.extension_module(
  'test_cyice',
  sources: [
    'src/test_cyice/test_cyice.pyx',
  ],
  dependencies: [py3_dep, np_dep, cspice_dep, m_dep],
  include_directories: [include_directories('src'),],
  install : true,
)
