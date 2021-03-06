PROJECT(navlib)

CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
CMAKE_POLICY(SET CMP0015 NEW)

SET(CMAKE_CXX_STANDARD 11)
SET(CMAKE_BUILD_TYPE Debug)

SET(CMAKE_CXX_FLAGS "-std=c++11 -O0 -fPIC")
SET(CMAKE_CXX_FLAGS "-msse3")
SET(CMAKE_C_FLAGS "-msse3")
SET(CMAKE_C_FLAGS "-mavx")

IF("${CMAKE_BUILD_TYPE}" STREQUAL "")
    SET(CMAKE_BUILD_TYPE "Release")
ENDIF()

INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/include)

FILE(GLOB SRC_FILES "src/ins-gnss/*.cc" "src/ins-gnss/rcv/*.cc")

ADD_LIBRARY(navlib ${SRC_FILES})
TARGET_LINK_LIBRARIES(navlib blas lapack z)

SET_TARGET_PROPERTIES(navlib PROPERTIES ARCHIVE_OUTPUT_DIRECTORY 
                      ${PROJECT_SOURCE_DIR}/lib)

SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY
    ${PROJECT_SOURCE_DIR}/bin)

ADD_EXECUTABLE(navapp src/ins-gnss/app/rtkrcv.cc)
TARGET_LINK_LIBRARIES(navapp navlib pthread)

ADD_EXECUTABLE(lc-rts src/ins-gnss/app/lc-rts.cc)
TARGET_LINK_LIBRARIES(lc-rts navlib pthread)

ADD_EXECUTABLE(lc-fbsm src/ins-gnss/app/lc-fbsm.cc)
TARGET_LINK_LIBRARIES(lc-fbsm navlib pthread)



