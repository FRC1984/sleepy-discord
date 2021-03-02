cp_flags = [
  '-DNONEXISTENT_UWEBSOCKETS=1',
  '-DEXISTENT_WEBSOCKETPP=1',
  '-DEXISTENT_OPUS=1',
  '-DEXISTENT_ASIO=1',
  '-DEXISTENT_CPR=1'
]

git = find_program('git')
use_src_dir = ['-C', meson.current_source_dir()]

cmds = [
  ['rev-list', '--count', 'HEAD'],
  ['rev-parse', '--abbrev-ref', 'HEAD'],
  ['rev-parse', 'HEAD'],
  ['describe', '--always', '--long', '--dirty']
]

res = []
foreach cmd: cmds
  r = run_command(git, '-C', meson.current_source_dir(), cmd, check: true)
  res += r.stdout().strip()
endforeach

if res[1] == 'master'
  sd_is_master = 1
else
  sd_is_master = 0
endif

if res[3] == ''
  sd_desc_concat = '""'
else
  sd_desc_concat = '" "'
endif

configure_file(
  output: 'version.h',
  configuration: {
    'SLEEPY_DISCORD_VERSION_BUILD': res[0],
    'SLEEPY_DISCORD_VERSION_BRANCH': '"' + res[1] + '"',
    'SLEEPY_DISCORD_VERSION_HASH': '"' + res[2] + '"',
    'SLEEPY_DISCORD_VERSION_IS_MASTER': sd_is_master,
    'SLEEPY_DISCORD_VERSION_DESCRIPTION_CONCAT': sd_desc_concat,
    'SLEEPY_DISCORD_VERSION_DESCRIPTION': '"' + res[3] + '"'
  }
)

version_inc = include_directories('.')

cp_lib = library('sleepy-discord',
  'assignments_client.cpp',
  'asio_udp.cpp',
  'attachment.cpp',
  'channel.cpp',
  'client.cpp',
  'cpr_session.cpp',
  'default_functions.cpp',
  'embed.cpp',
  'endpoints.cpp',
  'gateway.cpp',
  'http.cpp',
  'invite.cpp',
  'json_wrapper.cpp',
  'message.cpp',
  'permissions.cpp',
  'sd_error.cpp',
  'server.cpp',
  'user.cpp',
  'uwebsockets_websocket.cpp',
  'voice.cpp',
  'voice_connection.cpp',
  'webhook.cpp',
  'websocketpp_websocket.cpp',
  cpp_args: cp_flags,
  dependencies: [openssl, cpr, websocketpp, asio, opus],
  include_directories: ['../include/sleepy_discord', version_inc]
)

declare_dependency(
  compile_args: cp_flags,
  include_directories: '../include',
  link_with: cp_lib
)
