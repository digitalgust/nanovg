cmake_minimum_required(VERSION 3.8)
project(nanovg)

set(CMAKE_CXX_STANDARD 11)

add_definitions(-DNANOVG_GL2_IMPLEMENTATION)
include_directories(./deps/include/glad ./deps/include/ src)

set(SOURCE_FILES
        ./example/example_gl2.c
        ./example/demo.c
        ./example/perf.c
        src/nanovg.c
        ./deps/include/glad/glad.c
        )

add_executable(nanovg ${SOURCE_FILES})

IF (APPLE)
    message("64bit")
    find_library(MY_LIB libglfw3.a "../miniJVM/desktop/glfw_gui/c/deps/lib/mac/")
    find_library(OPENGL_LIB OpenGL)
    find_library(COCOA_LIB Cocoa)
    find_library(IOKIT_LIB IOKit)
    find_library(COREFOUNDATION_LIB CoreFoundation)
    find_library(COREVIDEO_LIB CoreVideo)
    TARGET_LINK_LIBRARIES(nanovg  ${MY_LIB} ${COCOA_LIB} ${IOKIT_LIB} ${OPENGL_LIB} ${COREFOUNDATION_LIB} ${COREVIDEO_LIB})
ELSEIF (MINGW)
    IF (CMAKE_SIZEOF_VOID_P EQUAL 4)
        message("32bit")
        find_library(MY_LIB libglfw3.a "../miniJVM/desktop/glfw_gui/c/deps/lib/win_i686/")
    elseif (CMAKE_SIZEOF_VOID_P EQUAL 8)
        message("64bit")
        find_library(MY_LIB libglfw3.a "../miniJVM/desktop/glfw_gui/c/deps/lib/win_x64/")
    endif ()

    TARGET_LINK_LIBRARIES(nanovg opengl32)
ENDIF ()

message(${MY_LIB})
MARK_AS_ADVANCED(MY_LIB)
TARGET_LINK_LIBRARIES(nanovg ${MY_LIB})