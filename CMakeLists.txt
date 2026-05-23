cmake_minimum_required(VERSION 3.25)
project(soundtouch~)

# ╭──────────────────────────────────────╮
# │               pd.cmake               │
# ╰──────────────────────────────────────╯
set(PDCMAKE_FILE ${CMAKE_BINARY_DIR}/pd.cmake)
if(NOT EXISTS "${PDCMAKE_FILE}")
    file(DOWNLOAD https://raw.githubusercontent.com/pure-data/pd.cmake/refs/tags/v2.0.15/pd.cmake ${PDCMAKE_FILE})
endif()
include(${PDCMAKE_FILE})

# ──────────────────────────────────────
set(CPM_FILE ${CMAKE_BINARY_DIR}/CPM.cmake)
set(CPM_VERSION "0.42.1")
if(NOT EXISTS "${CPM_FILE}")
    file(DOWNLOAD "https://github.com/cpm-cmake/CPM.cmake/releases/download/v${CPM_VERSION}/CPM.cmake" ${CPM_FILE})
endif()
include(${CPM_FILE})

# ──────────────────────────────────────
cpmaddpackage(
    NAME
    soundtouch
    GIT_REPOSITORY
    https://codeberg.org/soundtouch/soundtouch.git
    GIT_TAG
    2.4.1)

# ──────────────────────────────────────
set(STDIR ${soundtouch_SOURCE_DIR}/source/SoundTouch)
set(STSOURCES
    ${STDIR}/AAFilter.cpp
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
target_include_directories(soundtouch_tilde PRIVATE ${soundtouch_SOURCE_DIR}/include)
