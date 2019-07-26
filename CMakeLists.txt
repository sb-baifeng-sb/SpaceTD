cmake_minimum_required(VERSION 2.6)
project(SpaceTD)

# Use our modified FindSDL2* modules
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${SpaceTD_SOURCE_DIR}/cmake")
set(BIN_DIR ${SpaceTD_SOURCE_DIR}/bin)

# Bump up warning levels appropriately for clang, gcc & msvc and build in debug mode
if (${CMAKE_CXX_COMPILER_ID} STREQUAL "GNU" OR ${CMAKE_CXX_COMPILER_ID} STREQUAL "Clang")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -pedantic -std=c++14")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -g")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -O2")
elseif (${CMAKE_CXX_COMPILER_ID} STREQUAL "MSVC")
    if (CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
        string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
    else()
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
    endif()
endif()

find_package(SDL2 REQUIRED)
find_package(SDL2_Image REQUIRED)
find_package(SDL2_Mixer REQUIRED)
find_package(SDL2_ttf REQUIRED)
find_package(SDL2_gfx REQUIRED)
include_directories(${SDL2_INCLUDE_DIR} ${SDL2_IMAGE_INCLUDE_DIR} ${SDL2_MIXER_INCLUDE_DIR} ${SDL2_TTF_INCLUDE_DIR} ${SDL2_GFX_INCLUDE_DIR})

file(COPY resource DESTINATION .)

include_directories(include)
include_directories(/usr/local/Cellar/sdl2/2.0.9_1/include)
include_directories(/usr/local/Cellar/sdl2_ttf/2.0.15/include)
include_directories(/usr/local/Cellar/sdl2_mixer/2.0.4/include)
include_directories(/usr/local/Cellar/sdl2_image/2.0.5/include)
include_directories(/usr/local/Cellar/sdl2_gfx/1.0.4/include)

file(GLOB_RECURSE SpaceTD_SOURCES "src/*.c" "src/*.cc" "src/*.cpp")
add_executable (SpaceTD ${SpaceTD_SOURCES})

target_link_libraries(SpaceTD ${SDL2_LIBRARY} ${SDL2_IMAGE_LIBRARY} ${SDL2_MIXER_LIBRARY} ${SDL2_TTF_LIBRARY} ${SDL2_GFX_LIBRARY})

install(TARGETS SpaceTD RUNTIME DESTINATION ${BIN_DIR})
