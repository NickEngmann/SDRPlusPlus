cmake_minimum_required(VERSION 3.13)
project(limesdr_source)

if (MSVC)
    set(CMAKE_CXX_FLAGS "-O2 /std:c++17 /EHsc")
elseif (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    set(CMAKE_CXX_FLAGS "-O3 -std=c++17 -Wno-unused-command-line-argument -undefined dynamic_lookup")
else ()
    set(CMAKE_CXX_FLAGS "-O3 -std=c++17")
endif ()

include_directories("src/")

file(GLOB SRC "src/*.cpp")

add_library(limesdr_source SHARED ${SRC})
target_link_libraries(limesdr_source PRIVATE sdrpp_core)
set_target_properties(limesdr_source PROPERTIES PREFIX "")

if (MSVC)
    # Lib path
    target_link_directories(limesdr_source PUBLIC "C:/Program Files/PothosSDR/lib")

    target_link_libraries(limesdr_source PUBLIC LimeSuite)
else (MSVC)
    find_package(PkgConfig)

    # Not in pkg-config
    target_link_libraries(limesdr_source PUBLIC LimeSuite)

    # Include it because for some reason pkgconfig doesn't look here?
    if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
        target_include_directories(airspyhf_source PUBLIC "/usr/local/include")
    endif()
endif ()

# Install directives
install(TARGETS limesdr_source DESTINATION lib/sdrpp/plugins)