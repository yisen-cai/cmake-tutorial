cmake_minimum_required(VERSION 3.17)

project(condition-compilation)

# set variables
set(CMAKE_CXX_STANDARD 14)

set(USE_LIBRARY ON)

message(STATUS "Compile sources into a library? ${USE_LIBRARY}")

# defined in cmake, calling add_library and omitting the second argument will build a static library
set(BUILD_SHARED_LIBS OFF)


# listing message.h and message.cpp
# introduce a variable _sources
list(
    APPEND
    _sources

    src/message.h
    src/message.cpp
)

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