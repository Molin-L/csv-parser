project(AriaCsvParser)
cmake_minimum_required(VERSION 3.14 FATAL_ERROR)
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -stdlib=libc++")
add_library(${PROJECT_NAME} INTERFACE)

set_target_properties(${PROJECT_NAME} PROPERTIES
    PUBLIC_HEADER parser.hpp
)
enable_testing()

add_executable(${PROJECT_NAME}_test test/parser_test.cpp)
add_test(NAME ${PROJECT_NAME}_test COMMAND ${PROJECT_NAME}_test)
target_include_directories( ${PROJECT_NAME}
                INTERFACE
                $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
                $<INSTALL_INTERFACE:include>
                )

install(TARGETS ${PROJECT_NAME}
        EXPORT ${PROJECT_NAME}Targets)

install(EXPORT ${PROJECT_NAME}Targets
    NAMESPACE AriaCsvParser::
    FILE AriaCsvParserTargets.cmake
    DESTINATION lib/cmake/${PROJECT_NAME}
)

configure_package_config_file(Config.cmake.in 
    ${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION "lib/cmake/${PROJECT_NAME}"
    NO_SET_AND_CHECK_MACRO
    NO_CHECK_REQUIRED_COMPONENTS_MACRO
    )

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    DESTINATION lib/cmake/${PROJECT_NAME}
    )