add_library(
    mbgl-render-test STATIC EXCLUDE_FROM_ALL
    ${MBGL_ROOT}/expression-test/test_runner_common.cpp
    ${MBGL_ROOT}/expression-test/test_runner_common.hpp
    ${MBGL_ROOT}/render-test/allocation_index.cpp
    ${MBGL_ROOT}/render-test/allocation_index.hpp
    ${MBGL_ROOT}/render-test/file_source.cpp
    ${MBGL_ROOT}/render-test/file_source.hpp
    ${MBGL_ROOT}/render-test/filesystem.hpp
    ${MBGL_ROOT}/render-test/include/mbgl/render_test.hpp
    ${MBGL_ROOT}/render-test/manifest_parser.cpp
    ${MBGL_ROOT}/render-test/manifest_parser.hpp
    ${MBGL_ROOT}/render-test/metadata.hpp
    ${MBGL_ROOT}/render-test/parser.cpp
    ${MBGL_ROOT}/render-test/parser.hpp
    ${MBGL_ROOT}/render-test/render_test.cpp
    ${MBGL_ROOT}/render-test/runner.cpp
    ${MBGL_ROOT}/render-test/runner.hpp
)

if(CMAKE_SYSTEM_NAME STREQUAL iOS)
    set_target_properties(mbgl-render-test PROPERTIES XCODE_ATTRIBUTE_IPHONEOS_DEPLOYMENT_TARGET "${IOS_DEPLOYMENT_TARGET}")
    set_target_properties(mbgl-render-test PROPERTIES XCODE_ATTRIBUTE_ENABLE_BITCODE "YES")
    set_target_properties(mbgl-render-test PROPERTIES XCODE_ATTRIBUTE_BITCODE_GENERATION_MODE bitcode)
    set_target_properties(mbgl-render-test PROPERTIES XCODE_ATTRIBUTE_ONLY_ACTIVE_ARCH $<$<CONFIG:Debug>:YES>)
endif()

# FIXME: Should not use core private interface
target_include_directories(
    mbgl-render-test
    PRIVATE ${MBGL_ROOT}/src ${MBGL_ROOT}/platform/default/include
)

target_include_directories(
    mbgl-render-test
    PUBLIC ${MBGL_ROOT}/render-test/include ${MBGL_ROOT}/include
)

include(${PROJECT_SOURCE_DIR}/vendor/boost.cmake)

target_link_libraries(
    mbgl-render-test
    PRIVATE
        Mapbox::Base::Extras::args
        Mapbox::Base::Extras::filesystem
        Mapbox::Base::pixelmatch-cpp
        mbgl-compiler-options
        mbgl-vendor-boost
    PUBLIC mbgl-core
)

set_property(TARGET mbgl-render-test PROPERTY FOLDER Core)
