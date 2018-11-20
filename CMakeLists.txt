cmake_minimum_required (VERSION 3.10) # CMake version check
project (GnuPlotCpp) # Project Name
option(BUILD_EXAMPLE "Build example program" OFF)

find_program(GNU_PLOT_BIN gnuplot gnuplot/bin)
if(GNU_PLOT_BIN)
    message(STATUS "find gnuplot: ${GNU_PLOT_BIN}")
else()
    message(FATAL_ERROR "gnuplot not found")
endif()
add_library(gnuplot_i INTERFACE)
target_include_directories(gnuplot_i INTERFACE
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
) # add include directory to target
list(APPEND installed_targets gnuplot_i)

if(BUILD_EXAMPLE)
    add_executable(example example.cc) # An executable target
    target_compile_definitions(example PRIVATE GNU_PLOT_BIN="${GNU_PLOT_BIN}")
    target_link_libraries(example gnuplot_i)
    list(APPEND installed_targets example)
endif()

include(CMakePackageConfigHelpers)
set(config_install_path lib/cmake/${PROJECT_NAME})
configure_package_config_file(
    cmake/Config.cmake.in
    "${PROJECT_NAME}Config.cmake"
    INSTALL_DESTINATION ${config_install_path}
)
install(FILES "${CMAKE_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
    DESTINATION ${config_install_path}
 )
install( TARGETS ${installed_targets}
    EXPORT ${PROJECT_NAME}Targets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
install( EXPORT ${PROJECT_NAME}Targets
    NAMESPACE ${PROJECT_NAME}::
    DESTINATION ${config_install_path}
)
install(
    FILES gnuplot_i.hpp
    DESTINATION ${CMAKE_INSTALL_PREFIX}/include
)