project('CSPICE', 'c',
  version : get_option('cspice_version'),
  meson_version : '>=0.55.0'
)
# get basic info
host = host_machine()
system = host.system()
arch = host.cpu_family()
# get the compiler
cc = meson.get_compiler('c')
# Log the compiler
message('Using C Compiler: ' + cc.get_id())
message('C Compiler family: ' + arch)

# determine the build to run
if system == 'windows'
  wrap_name = 'mylib-windows'
elif system == 'darwin'
  wrap_name = 'mylib-macos'
else
  wrap_name = 'mylib-linux'
endif


# get the math library if needed
m_dep = cc.find_library('m', required : false)

