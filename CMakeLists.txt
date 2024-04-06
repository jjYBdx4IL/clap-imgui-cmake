cmake_minimum_required(VERSION 3.22)

if(UNIX)
    cmake_path(SET USERHOME NORMALIZE $ENV{HOME})
else()
    cmake_path(SET USERHOME NORMALIZE $ENV{USERPROFILE})
endif()

#
# USER CONFIG
#

# your reaper install directory for debugging
# (best to use a separate and clean portable install that's user-writable)
cmake_path(SET REAPER_HOME NORMALIZE "${USERHOME}/source/reaper")

#
# THE PROJECT
#
project(clap-imgui-cmake C CXX)
set(CMAKE_CXX_STANDARD 20)


if (MSVC)
  set(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "" FORCE)
  add_compile_options(/MP)
  set_property(GLOBAL PROPERTY USE_FOLDERS ON)
  add_compile_definitions(NOMINMAX UNICODE)
endif()

cmake_path(SET REAPER_EXE_PATH NORMALIZE "${REAPER_HOME}/reaper.exe")

set(SRCDIR "clap-imgui/src")
add_library(clap-imgui SHARED
    ${SRCDIR}/glfw/src/context.c
    ${SRCDIR}/glfw/src/egl_context.c
    ${SRCDIR}/glfw/src/init.c
    ${SRCDIR}/glfw/src/input.c
    ${SRCDIR}/glfw/src/monitor.c
    ${SRCDIR}/glfw/src/null_init.c
    ${SRCDIR}/glfw/src/null_joystick.c
    ${SRCDIR}/glfw/src/null_monitor.c
    ${SRCDIR}/glfw/src/null_window.c
    ${SRCDIR}/glfw/src/osmesa_context.c
    ${SRCDIR}/glfw/src/platform.c
    ${SRCDIR}/glfw/src/vulkan.c
    ${SRCDIR}/glfw/src/wgl_context.c
    ${SRCDIR}/glfw/src/win32_init.c
    ${SRCDIR}/glfw/src/win32_joystick.c
    ${SRCDIR}/glfw/src/win32_module.c
    ${SRCDIR}/glfw/src/win32_monitor.c
    ${SRCDIR}/glfw/src/win32_thread.c
    ${SRCDIR}/glfw/src/win32_time.c
    ${SRCDIR}/glfw/src/win32_window.c
    ${SRCDIR}/glfw/src/window.c
    ${SRCDIR}/gui.cpp
    ${SRCDIR}/imgui/imgui_knob.cpp
    ${SRCDIR}/imgui_base.cpp
    ${SRCDIR}/imgui/backends/imgui_impl_glfw.cpp
    ${SRCDIR}/imgui/backends/imgui_impl_opengl3.cpp
    ${SRCDIR}/imgui/imgui.cpp
    ${SRCDIR}/imgui/imgui_demo.cpp
    ${SRCDIR}/imgui/imgui_draw.cpp
    ${SRCDIR}/imgui/imgui_tables.cpp
    ${SRCDIR}/imgui/imgui_widgets.cpp
    ${SRCDIR}/imgui_win.cpp
    ${SRCDIR}/main.cpp
    ${SRCDIR}/plugin.cpp
    ${SRCDIR}/plugin_impl_0.cpp
    ${SRCDIR}/plugin_impl_1.cpp
    ${SRCDIR}/clap/include/clap/audio-buffer.h
    ${SRCDIR}/clap/include/clap/clap.h
    ${SRCDIR}/clap/include/clap/color.h
    ${SRCDIR}/clap/include/clap/entry.h
    ${SRCDIR}/clap/include/clap/events.h
    ${SRCDIR}/clap/include/clap/ext/audio-ports-config.h
    ${SRCDIR}/clap/include/clap/ext/audio-ports.h
    ${SRCDIR}/clap/include/clap/ext/event-registry.h
    ${SRCDIR}/clap/include/clap/ext/gui.h
    ${SRCDIR}/clap/include/clap/ext/latency.h
    ${SRCDIR}/clap/include/clap/ext/log.h
    ${SRCDIR}/clap/include/clap/ext/note-name.h
    ${SRCDIR}/clap/include/clap/ext/note-ports.h
    ${SRCDIR}/clap/include/clap/ext/params.h
    ${SRCDIR}/clap/include/clap/ext/posix-fd-support.h
    ${SRCDIR}/clap/include/clap/ext/render.h
    ${SRCDIR}/clap/include/clap/ext/state.h
    ${SRCDIR}/clap/include/clap/ext/thread-check.h
    ${SRCDIR}/clap/include/clap/ext/thread-pool.h
    ${SRCDIR}/clap/include/clap/ext/timer-support.h
    ${SRCDIR}/clap/include/clap/fixedpoint.h
    ${SRCDIR}/clap/include/clap/host.h
    ${SRCDIR}/clap/include/clap/id.h
    ${SRCDIR}/clap/include/clap/plugin-factory.h
    ${SRCDIR}/clap/include/clap/plugin-invalidation.h
    ${SRCDIR}/clap/include/clap/plugin.h
    ${SRCDIR}/clap/include/clap/private/macros.h
    ${SRCDIR}/clap/include/clap/private/std.h
    ${SRCDIR}/clap/include/clap/process.h
    ${SRCDIR}/clap/include/clap/stream.h
    ${SRCDIR}/clap/include/clap/string-sizes.h
    ${SRCDIR}/clap/include/clap/version.h
    ${SRCDIR}/glfw/src/internal.h
    ${SRCDIR}/glfw/src/mappings.h
    ${SRCDIR}/glfw/src/null_joystick.h
    ${SRCDIR}/glfw/src/null_platform.h
    ${SRCDIR}/glfw/src/platform.h
    ${SRCDIR}/glfw/src/win32_joystick.h
    ${SRCDIR}/glfw/src/win32_platform.h
    ${SRCDIR}/glfw/src/win32_thread.h
    ${SRCDIR}/glfw/src/win32_time.h
    ${SRCDIR}/imgui/backends/imgui_impl_glfw.h
    ${SRCDIR}/imgui/backends/imgui_impl_opengl3_loader.h
    ${SRCDIR}/imgui/imconfig.h
    ${SRCDIR}/imgui/imgui.h
    ${SRCDIR}/imgui/imgui_internal.h
    ${SRCDIR}/imgui/imstb_rectpack.h
    ${SRCDIR}/imgui/imstb_textedit.h
    ${SRCDIR}/imgui/imstb_truetype.h
    ${SRCDIR}/main.h
)
set_target_properties(clap-imgui PROPERTIES SUFFIX ".clap")

add_compile_definitions(WIN32 CLAPPLUGIN_EXPORTS _GLFW_WIN32 _WINDOWS _USRDLL GLFW_EXPOSE_NATIVE_WIN32)

target_include_directories(clap-imgui PRIVATE ${SRCDIR}/imgui ${SRCDIR}/glfw/include)

set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT clap-imgui)

#
# set up debugging under reaper for convenience
#
if (EXISTS "${REAPER_EXE_PATH}")
    set_target_properties(clap-imgui PROPERTIES
        VS_DEBUGGER_COMMAND "${REAPER_EXE_PATH}"
        VS_DEBUGGER_COMMAND_ARGUMENTS ""
    )
endif()


