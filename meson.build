project(
    'test_cyice', 
    'c', 
    'cython',
    default_options : ['warning_level=3', 'buildtype=release'],
    license : 'MIT',
    license_files: 'LICENSE',
    version: '7.1.0',
    meson_version: '>=1.2.0'
    )

# python stuff
py_mod = import('python')
py = py_mod.find_installation(pure: false)
py_dep = py.dependency(required: true)
py_install_dir = py.get_install_dir(subdir: 'test_cyice/utils', pure: false) 
message('py install dir: ', py_install_dir)

# cython stuff
cy = meson.get_compiler('cython')

# c stuff
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

# cspice stuff
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

message('install_dir: ', py_install_dir)

libcspice_sub = subproject(
  cspice_wrap_name,
  default_options: {
    'only_install_main_lib': true,
    'install_dir': py_install_dir,
  }
)
cspice_dep = libcspice_sub.get_variable('cspice_dep')
cspice_shared = libcspice_sub.get_variable('libcspice')
cspice_headers = libcspice_sub.get_variable('cspice_inc')

## numpy stuff
# get base numpy dep
base_numpy_dep = dependency('numpy', required: true)
# declare numpy dep
numpy_dep = declare_dependency(
  dependencies: base_numpy_dep,
  compile_args: ['-DNPY_NO_DEPRECATED_API=NPY_1_9_API_VERSION']
)


subdir('src/test_cyice')