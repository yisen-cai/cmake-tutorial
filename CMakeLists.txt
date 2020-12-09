cmake_minimum_required(VERSION 3.17)

project(build-type)

# set variables
set(CMAKE_CXX_STANDARD 14)


if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()

message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "C flags, Debug configuration: ${CMAKE_C_FLAGS_DEBUG}")
message(STATUS "C flags, Release configuration: ${CMAKE_C_FLAGS_RELEASE}")
message(STATUS "C flags, Release configuration with Debug info: ${CMAKE_C_FLAGS_RELWITHDEBIFO}")


# listing message.h and message.cpp
# introduce a variable _sources
list(
    APPEND
    _sources

    src/message.h
    src/message.cpp
)

if(MAKE_SHARED_LIBRARY)

    message(STATUS "Make shared library")
    add_library(
        message-shared
        SHARED

        ${_sources}
    )
endif(MAKE_SHARED_LIBRARY)

if(USE_LIBRARY)
    add_library(
        message

        ${_sources}
    )

    message(STATUS "Build with static library")

    add_executable(condition-compilation  src/main.cpp)

    target_link_libraries(condition-compilation message )

else()
    message(STATUS "Build without static library")
    # else not use static library
    add_executable(condition-compilation src/main.cpp ${_sources})

endif(USE_LIBRARY)