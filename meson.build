project('test-cspice', 'c')

cspice_dep = dependency(
    'libcspice', 
    fallback: 'libcspice-macos-arm'
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
