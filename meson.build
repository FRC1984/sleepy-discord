project('sleepy-discord',
  'cpp',
  meson_version: '>=0.55.0',
  default_options: [
    'cpp_std=c++17',
    'buildtype=release',
    'b_ndebug=if-release',
    'warning_level=3',
    'b_lto=true',
    'pkg_config_path=' + meson.current_build_dir()
  ]
)

if get_option('use_conan')
  conan = find_program('conan')

  if get_option('buildtype').startswith('release')
    conan_bt = 'build_type=Release'
  else
    conan_bt = 'build_type=Debug'
  endif

  r = run_command(conan,
    'install', meson.current_source_dir(),
    '-if', meson.current_build_dir(),
    '-s', conan_bt,
    '--build=missing'
  )

  if r.returncode() != 0
    error('Failed to install dependencies with Conan')
  endif
endif

openssl = dependency('openssl', include_type: 'system')
cpr = dependency('cpr', include_type: 'system')
websocketpp = dependency('websocketpp', include_type: 'system')
asio = dependency('asio', include_type: 'system')
opus = dependency('opus', include_type: 'system')

subdir('sleepy_discord')
