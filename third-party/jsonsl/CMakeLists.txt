add_library(jsonsl STATIC
    jsonsl.c
    jsonsl.h)

if(NOT WIN32)
    add_definitions (-fPIC)
endif()

target_include_directories(jsonsl
    PUBLIC
        ${CMAKE_CURRENT_LIST_DIR})
