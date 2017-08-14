cmake_minimum_required(VERSION 3.1)

project(CommandCenter)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin)

set(SC2Api_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/../SC2Api/include")
set(SC2Api_LIBRARIES
        "${CMAKE_CURRENT_SOURCE_DIR}/../s2client-api/build/bin/libcivetweb.a"
        "${CMAKE_CURRENT_SOURCE_DIR}/../s2client-api/build/bin/libprotobuf-lite.a"
        "${CMAKE_CURRENT_SOURCE_DIR}/../s2client-api/build/bin/libprotobuf.a"
        "${CMAKE_CURRENT_SOURCE_DIR}/../s2client-api/build/bin/libprotoc.a"
        "${CMAKE_CURRENT_SOURCE_DIR}/../s2client-api/build/bin/libsc2api.a"
        "${CMAKE_CURRENT_SOURCE_DIR}/../s2client-api/build/bin/libsc2lib.a"
        "${CMAKE_CURRENT_SOURCE_DIR}/../s2client-api/build/bin/libsc2protocol.a"
        "${CMAKE_CURRENT_SOURCE_DIR}/../s2client-api/build/bin/libsc2utils.a"
        )

# Build with c++14 support, required by sc2api.
set(CMAKE_CXX_STANDARD 14)

include_directories("${SC2Api_INCLUDE_DIRS}")

# All the source files for the bot.
file(GLOB BOT_SOURCES "src/*.cpp" "src/*.h" "src/*.hpp")

# Show more warnings at compiletime.
if (MSVC)
    # FIXME: put Windows specific options here.
else ()
    set(CMAKE_CXX_FLAGS "-Wall -Wextra")
endif()

if (APPLE)
    # Supress annoying 'deprecated' warnings caused by sc2api headers.
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-deprecated")
endif ()

# Create the executable.
add_executable(CommandCenter ${BOT_SOURCES})
target_link_libraries(CommandCenter ${SC2Api_LIBRARIES})

if (APPLE)
    target_link_libraries(CommandCenter "-framework Carbon")
endif ()