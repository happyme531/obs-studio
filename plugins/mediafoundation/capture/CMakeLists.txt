cmake_minimum_required(VERSION 3.28...3.30)

find_package(FFmpeg REQUIRED avcodec avutil)
find_package(WIL REQUIRED)

add_library(win-mf-capture MODULE)
add_library(OBS::mfcapture ALIAS win-mf-capture)

target_sources(win-mf-capture PUBLIC mf-plugin.cpp win-mf.cpp)

add_library(libmfcapture INTERFACE)
add_library(OBS::libmfcapture ALIAS libmfcapture)

target_include_directories(libmfcapture INTERFACE libmfcapture libmfcapture/source)

target_sources(
  libmfcapture
  INTERFACE
    libmfcapture/source/CriticalSection.cpp
    libmfcapture/source/DeviceControlChangeListener.cpp
    libmfcapture/source/MediaFoundationSourceInput.cpp
    libmfcapture/source/mfcapture.cpp
    libmfcapture/source/PhysicalCamera.cpp
    libmfcapture/source/VideoBufferLock.cpp
)

configure_file(cmake/windows/obs-module.rc.in win-mf-capture-module.rc)
configure_file(libmfcapture/source/mfcapture.rc.in win-mf-capture-res.rc)
target_sources(win-mf-capture PRIVATE win-mf-capture-module.rc win-mf-capture-res.rc)

target_precompile_headers(win-mf-capture PRIVATE <wil/cppwinrt.h> <wil/result.h> <wil/com.h>)

target_link_libraries(
  win-mf-capture
  PRIVATE
    OBS::libobs
    OBS::w32-pthreads
    OBS::winrt-headers
    FFmpeg::avcodec
    FFmpeg::avutil
    strmiids
    winmm
    dxcore
    dxguid
    libmfcapture
)

set_target_properties_obs(win-mf-capture PROPERTIES FOLDER plugins/win-mf-capture PREFIX "")
