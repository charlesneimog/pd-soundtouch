cmake_minimum_required(VERSION 3.25)

project(soundtouch~)

#╭──────────────────────────────────────╮
#│               pd.cmake               │
#╰──────────────────────────────────────╯
cmake_policy(SET CMP0135 NEW)
set(PDCMAKE_FILE ${CMAKE_BINARY_DIR}/pd.cmake)
if(NOT EXISTS ${PDCMAKE_FILE})
    message(STATUS "Downloading pd.cmake")
    file(
        DOWNLOAD https://raw.githubusercontent.com/pure-data/pd.cmake/refs/tags/v0.1.0/pd.cmake ${PDCMAKE_FILE}
        SHOW_PROGRESS
        STATUS DOWNLOAD_STATUS)
endif()
include(${PDCMAKE_FILE})

set(STDIR ${CMAKE_CURRENT_SOURCE_DIR}/resources/soundtouch/source/SoundTouch)
set(STSOURCES ${STDIR}/AAFilter.cpp
              ${STDIR}/cpu_detect_x86.cpp
              ${STDIR}/mmx_optimized.cpp
              ${STDIR}/FIFOSampleBuffer.cpp
              ${STDIR}/RateTransposer.cpp
              ${STDIR}/FIRFilter.cpp
              ${STDIR}/InterpolateCubic.cpp
              ${STDIR}/InterpolateLinear.cpp
              ${STDIR}/InterpolateShannon.cpp
              ${STDIR}/sse_optimized.cpp
              ${STDIR}/TDStretch.cpp
              ${STDIR}/SoundTouch.cpp
              src/soundtouch~.cpp)


pd_add_external(soundtouch~ "${STSOURCES}")
pd_add_datafile(soundtouch~ "${CMAKE_CURRENT_SOURCE_DIR}/soundtouch~-help.pd")
target_include_directories(soundtouch_tilde PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/resources/soundtouch/include)






