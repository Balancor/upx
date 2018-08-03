cmake_minimum_required(VERSION 3.9)
project(upx)

set(CMAKE_CXX_STANDARD 11)
SET(UPX_CXX_FLAGS "-O0 -g ")
string(APPEND UPX_CXX_FLAGS " -fno-strict-aliasing -fwrapv")
string(APPEND UPX_CXX_FLAGS " -funsigned-char")
string(APPEND UPX_CXX_FLAGS " -Wall -W -Wcast-align -Wcast-qual -Wmissing-declarations")
string(APPEND UPX_CXX_FLAGS "  -Wpointer-arith -Wshadow -Wvla -Wwrite-strings  -Wno-shadow")
string(APPEND UPX_CXX_FLAGS " -Werror")
string(APPEND CMAKE_C_FLAGS ${UPX_CXX_FLAGS})
string(APPEND CMAKE_CXX_FLAGS ${UPX_CXX_FLAGS})

include_directories(src/lzma-sdk)
include_directories(lib/include)

INCLUDE(FindZLIB)
include_directories(${ZLIB_INCLUDE_DIR})
link_directories(${ZLIB_LIBRARIES})

add_library(ucl STATIC IMPORTED)
SET_TARGET_PROPERTIES(ucl PROPERTIES IMPORTED_LOCATION
        ${PROJECT_SOURCE_DIR}/lib/lib/libucl.a)

SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/out)
SET(LIBRARY_OUTPUT_PATH  ${PROJECT_SOURCE_DIR}/out)

SET(UPX_SOURCES
    src/c_file.cpp
    src/c_init.cpp
    src/c_none.cpp
    src/compress.cpp
    src/compress_lzma.cpp
    src/compress_ucl.cpp
    src/compress_zlib.cpp
    src/c_screen.cpp
    src/except.cpp
    src/file.cpp
    src/filter.cpp
    src/filteri.cpp
    src/help.cpp
    src/lefile.cpp
    src/linker.cpp
    src/main.cpp
    src/mem.cpp
    src/msg.cpp
    src/packer_c.cpp
    src/packer.cpp
    src/packer_f.cpp
    src/packhead.cpp
    src/packmast.cpp
    src/p_armpe.cpp
    src/p_com.cpp
    src/p_djgpp2.cpp
    src/pefile.cpp
    src/p_exe.cpp
    src/p_lx_elf.cpp
    src/p_lx_exc.cpp
    src/p_lx_interp.cpp
    src/p_lx_sh.cpp
    src/p_mach.cpp
    src/p_ps1.cpp
    src/p_sys.cpp
    src/p_tmt.cpp
    src/p_tos.cpp
    src/p_unix.cpp
    src/p_vmlinx.cpp
    src/p_vmlinz.cpp
    src/p_w32pe.cpp
    src/p_w64pep.cpp
    src/p_wcle.cpp
    src/s_djgpp2.cpp
    src/snprintf.cpp
    src/s_object.cpp
    src/stdcxx.cpp
    src/s_vcsa.cpp
    src/s_win32.cpp
    src/ui.cpp
    src/util.cpp
    src/work.cpp)

add_executable(upx ${UPX_SOURCES})

target_link_libraries(upx ucl z)