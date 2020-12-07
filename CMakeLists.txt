cmake_minimum_required(VERSION 3.17)
project(static-library)

# set variables
set(CMAKE_CXX_STANDARD 14)


# obj can be used to compile the sources in the list given to `add_library` to object files
# The use of libraries particularly useful if one need to create both static and shared libraries in one go.
add_library(
    message-objs
    OBJECT

    src/message.h
    src/message.cpp
    )

# this is only needed for older compilers
# but doesn't hurt either to have it
set_target_properties(
    message-objs
    PROPERTIES
    POSITION_INDEPENDENT_CODE 1
)

# Libraries that can be linked dynamically and loaded at runtime.
add_library(
    message-shared
    SHARED
    $<TARGET_OBJECTS:message-objs>
)

# Used to create static libraries, archives of object files for used when linking other targets(executables).
add_library(
    message-static
    STATIC
    $<TARGET_OBJECTS:message-objs>
)



# MODULE libraries are one again DSOs, are not linked to any other target withen the project, but may be loaded dynamically later on.



add_executable(hello-world src/main.cpp)

target_link_libraries(hello-world message-static)