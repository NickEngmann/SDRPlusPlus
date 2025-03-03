cmake_minimum_required(VERSION 3.13)
project(network_sink)

if (MSVC)
    set(CMAKE_CXX_FLAGS "-O2 /std:c++17 /EHsc")
elseif (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    set(CMAKE_CXX_FLAGS "-O3 -std=c++17 -Wno-unused-command-line-argument -undefined dynamic_lookup")
else ()
    set(CMAKE_CXX_FLAGS "-O3 -std=c++17")
endif ()

file(GLOB SRC "src/*.cpp")

include_directories("src/")
include_directories("../recorder/src")
include_directories("../meteor_demodulator/src")
include_directories("../radio/src")

add_library(network_sink SHARED ${SRC})
target_link_libraries(network_sink PRIVATE sdrpp_core)
set_target_properties(network_sink PROPERTIES PREFIX "")

# Install directives
install(TARGETS network_sink DESTINATION lib/sdrpp/plugins)