project(
  '4jcraft',
  ['cpp', 'c'],
  version: '0.1.0',
  meson_version: '>= 1.7',
  default_options: [
    'cpp_std=c++23',
    'warning_level=0',
    'buildtype=debug',
    'unity=on',
    'unity_size=8',
    'b_pch=true',
  ],
)

pymod = import('python')
python = pymod.find_installation('python3', required: true)

cc = meson.get_compiler('cpp')

use_gles = get_option('renderer') == 'gles'

# -----------------------------
# GLOBAL DEFINES
# -----------------------------
global_cpp_defs = [
  '-DSPLIT_SAVES',
  '-D_LARGE_WORLDS',
  '-D_EXTENDED_ACHIEVEMENTS',
  '-D_DEBUG_MENUS_ENABLED',
  '-D_DEBUG',
  '-DDEBUG',
]

if host_machine.system() == 'linux'
  global_cpp_defs += ['-Dlinux', '-D__linux', '-D__linux__']
endif

if host_machine.system() == 'emscripten'
  global_cpp_defs += ['-D__EMSCRIPTEN__']
endif

if use_gles
  global_cpp_defs += ['-DGLES']
endif

add_project_arguments(global_cpp_defs, language: ['cpp', 'c'])

global_cpp_args = [
  '-fpermissive',
  '-Wshift-count-overflow',
  '-pipe',
]

add_project_arguments(global_cpp_args, language: 'cpp')

# -----------------------------
# DEPENDENCIES (CROSS SAFE)
# -----------------------------

# SDL2 (IMPORTANT FIX)
if host_machine.system() == 'emscripten'
  # Provided by emscripten (-sUSE_SDL=2)
  sdl2_dep = declare_dependency()
else
  sdl2_dep = dependency('sdl2')
endif

thread_dep = dependency('threads')

# dl is NOT available in WASM
if host_machine.system() == 'emscripten'
  dl_dep = declare_dependency()
else
  dl_dep = cc.find_library('dl', required: true)
endif

# -----------------------------
# OPENGL / GLES
# -----------------------------
if use_gles
  gl_dep = dependency('glesv2', required: true)
  glu_dep = declare_dependency()
else
  gl_dep = dependency('gl', required: true)
  glu_dep = dependency('glu', required: true)
endif

# -----------------------------
# SUBPROJECTS
# -----------------------------
stb = subproject('stb').get_variable('stb_inc')
stb_dep = declare_dependency(include_directories: stb)

miniaudio_dep = dependency('miniaudio')

# -----------------------------
# BUILD TREE
# -----------------------------
subdir('4J.Render')
subdir('4J.Input')
subdir('4J.Profile')
subdir('4J.Storage')

subdir('Minecraft.Assets')
subdir('Minecraft.World')
subdir('Minecraft.Client')
