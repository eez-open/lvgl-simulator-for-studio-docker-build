cmake_minimum_required(VERSION 3.12)
project (eez-studio-lvgl-simulator)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -s USE_SDL=2")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -s USE_SDL=2")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -s INITIAL_MEMORY=83886080 -sLLD_REPORT_UNDEFINED -s USE_SDL=2 -L/opt/freetype-2.14.1/build -lfreetype")

add_compile_options(-Wno-unknown-warning-option)

add_definitions(-DEEZ_LVGL_SIMULATOR)

# Add display dimensions as compile definitions if provided via environment
if(DEFINED ENV{DISPLAY_WIDTH})
    add_definitions(-DDISPLAY_WIDTH=$ENV{DISPLAY_WIDTH})
endif()
if(DEFINED ENV{DISPLAY_HEIGHT})
    add_definitions(-DDISPLAY_HEIGHT=$ENV{DISPLAY_HEIGHT})
endif()

# Add compile definitions if provided via environment
if(DEFINED ENV{ENCODER_GROUP})
    add_definitions(-DENCODER_GROUP=$ENV{ENCODER_GROUP})
endif()
if(DEFINED ENV{KEYBOARD_GROUP})
    add_definitions(-DKEYBOARD_GROUP=$ENV{KEYBOARD_GROUP})
endif()

include_directories(
    ${PROJECT_SOURCE_DIR}
    /opt/freetype-2.14.1/include
)

set(CONFIG_LV_BUILD_DEMOS OFF CACHE BOOL "" FORCE)
set(CONFIG_LV_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(CONFIG_LV_USE_THORVG_INTERNAL OFF CACHE BOOL "" FORCE)

add_subdirectory(lvgl)

file(GLOB MY_SOURCES "./*.c")
set(SOURCES ${MY_SOURCES})

file(GLOB_RECURSE my_src
    ./src/*.cpp
    ./src/*.c
)
list(APPEND SOURCES ${my_src})

add_executable(index ${SOURCES})

set(CMAKE_EXECUTABLE_SUFFIX ".html")

target_link_libraries(index
    lvgl
)

set_target_properties(index PROPERTIES LINK_FLAGS "--shell-file ${PROJECT_SOURCE_DIR}/index.html")
