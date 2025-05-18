project(
    'test-cspice', 
    'c',
    meson_version: '>=0.63.0'
)

# get basic info
host = host_machine
system = host.system()
arch = host.cpu_family()
is_unix    = system == 'linux'
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

# locate math lib if you want a proper dependency object (optional)
m_dep = cc.find_library('m', required: false)

# Build the test executable
test_exe = executable(
    'test_cspice', 
    'test_cspice.c',
    dependencies      : [ cspice_dep, m_dep ],
    install: false
)

test('cspice sanity', test_exe)
