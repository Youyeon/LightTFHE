cmake_minimum_required(VERSION 3.0)

set(SRCS
    lwekey.cpp
    lwe-functions.cpp
    lweparams.cpp
    lwesamples.cpp
    numeric-functions.cpp
)

add_library(tfhe STATIC ${SRCS} ${TFHE_HEADERS})
set_property(TARGET tfhe PROPERTY DIRECTORY "~/mytfhe/lib")
set_property(TARGET tfhe PROPERTY POSITION_INDEPENDENT_CODE ON)