set(TARGET jsobjects_jsc)

include_directories(
  ${PROJECT_SOURCE_DIR}/include
  ${JSC_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
)

if (NOT APPLE AND UNIX)
add_definitions(
  -fPIC
)
endif ()

add_library(${TARGET} ${LIBRARY_TYPE}
  ${PROJECT_SOURCE_DIR}/include/jsobjects.hpp
  ${PROJECT_SOURCE_DIR}/include/jsobjects_jsc.hpp
  jsobjects_jsc.cxx
)

target_link_libraries(${TARGET}
  ${JSC_LIBRARIES}
  ${JSC}
)
