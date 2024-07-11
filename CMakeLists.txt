cmake_minimum_required(VERSION 3.15)
project(lua VERSION 5.1.5 LANGUAGES C)

include(GNUInstallDirs) # CMAKE_INSTALL_LIBDIR, _INCLUDEDIR, etc.
include(CMakePackageConfigHelpers)  # configure_package_config_file

add_library(lua STATIC
    # core components
    src/lapi.c
    src/lapi.h
    src/lcode.c
    src/lcode.h
    src/ldebug.c
    src/ldebug.h
    src/ldo.c
    src/ldo.h
    src/ldump.c
    src/lfunc.c
    src/lfunc.h
    src/lgc.c
    src/lgc.h
    src/llex.c
    src/llex.h
    src/lmem.c
    src/lmem.h
    src/lobject.c
    src/lobject.h
    src/lopcodes.c
    src/lopcodes.h
    src/lparser.c
    src/lparser.h
    src/lstate.c
    src/lstate.h
    src/lstring.c
    src/lstring.h
    src/ltable.c
    src/ltable.h
    src/ltm.c
    src/ltm.h
    src/lundump.c
    src/lundump.h
    src/lvm.c
    src/lvm.h
    src/lzio.c
    src/lzio.h

    # library components
    src/lauxlib.c
    src/lauxlib.h
    src/ldblib.c
    src/liolib.c
    src/lmathlib.c
    src/loslib.c
    src/ltablib.c
    src/lstrlib.c
    src/loadlib.c
    src/linit.c
)
target_include_directories(lua INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

# add `luac` optional binary (handy for checking if `lua` can be linked etc.)
add_executable(luac EXCLUDE_FROM_ALL src/luac.c src/print.c)
target_link_libraries(luac lua)

# install the `lua` binaries (to the binary dir) and the headers to the
# installed header dir
if(TRUE)
    install(TARGETS lua EXPORT luaTargets)
    install(
        DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/src
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        FILES_MATCHING PATTERN "*.h"
    )
endif()
# generate `luaConfig.cmake` and `luaTargets.cmake` so that downstream
# cmake builds can use `find_package` to find the installed binaries/headers
if(TRUE)
    install(
        EXPORT luaTargets
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/lua
    )
    configure_package_config_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/luaConfig.cmake.in
        ${CMAKE_CURRENT_BINARY_DIR}/luaConfig.cmake
        INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/lua
    )
    install(
        FILES ${CMAKE_CURRENT_BINARY_DIR}/luaConfig.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/lua
    )
endif()

