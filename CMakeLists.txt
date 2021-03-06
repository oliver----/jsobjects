PROJECT(jsobjects)
CMAKE_MINIMUM_REQUIRED(VERSION 2.8.4)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/config)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/swig)

set(EXTERNALS_ONLY OFF CACHE BOOL "Set this if you want to build externals only. (ON|OFF)")
set(EXTERNALS_DIR ${PROJECT_SOURCE_DIR}/ext CACHE STRING "Directory where external projects should be downloaded to" )
set(LIBRARY_TYPE "SHARED" CACHE STRING "Type of library to be created (SHARED|STATIC)")

set(ENABLE_JSC OFF CACHE BOOL "If JavascriptCore adapter should be created (ON|OFF)")
set(ENABLE_V8 OFF CACHE BOOL "If V8 adapter should be created (ON|OFF)")
set(ENABLE_CPP OFF CACHE BOOL "If C++ adapter should be created (ON|OFF)")

set(ENABLE_TESTS OFF CACHE BOOL "If test suite should be built (ON|OFF)")
set(ENABLE_EXAMPLES OFF CACHE BOOL "If examples should be built (ON|OFF)")

set(SWIG_COMMAND "" CACHE STRING "swig executable.")
set(SWIG_TARGET "jsc" CACHE STRING "swig javasript target (jsc|v8)")

set(jsobjects_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/include)

if(ENABLE_EXAMPLES)
  if(NOT EXISTS SWIG_COMMAND)
    message (FATAL_ERROR "Mandatory: path to swig executable (or preinst-swig).")
  endif()
  set(ENABLE_SWIG ON)
endif()

# Configure external projects
# ===========================
if (EXTERNALS_ONLY OR IMPORT)

  set (DOWNLOAD_EXTERNALS ON)

endif()

include(Externals)

if (NOT EXTERNALS_ONLY OR IMPORT)

  include(ExportTarget)

  if (ENABLE_CPP)
    add_subdirectory(src/cpp)
  endif ()

  if (ENABLE_JSC)
    add_subdirectory(src/jsc)
  endif ()

  if (ENABLE_V8)
    add_subdirectory(src/v8)
  endif ()

  if (ENABLE_EXAMPLES)
    if (ENABLE_SWIG)
      add_subdirectory(examples/swig)
    endif ()
  endif ()

  if (ENABLE_TESTS)
    enable_testing()
    add_subdirectory(test)
  endif ()

endif()
