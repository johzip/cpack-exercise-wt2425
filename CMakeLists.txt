cmake_minimum_required(VERSION "3.28")

project("cpackexample" VERSION 0.1.0)

# Adding cpack module
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Include module
include(CPackConfig)


FIND_PACKAGE(deal.II 9.5 REQUIRED
  HINTS ${DEAL_II_DIR} ../ ../../ $ENV{DEAL_II_DIR}
  )
DEAL_II_INITIALIZE_CACHED_VARIABLES()

# Uses the FindBoost module of CMake
find_package(Boost 1.83 COMPONENTS filesystem REQUIRED)

find_package(yaml-cpp 0.6 REQUIRED)

add_library(cpackexamplelib filesystem/filesystem.cpp fem/fem.cpp flatset/flatset.cpp yamlParser/yamlParser.cpp)
add_executable("${PROJECT_NAME}" main.cpp)


target_link_libraries("${PROJECT_NAME}" cpackexamplelib)
target_link_libraries(cpackexamplelib Boost::filesystem ${YAML_CPP_LIBRARIES})
set_target_properties(cpackexamplelib PROPERTIES PUBLIC_HEADER "fem/fem.hpp;filesystem/filesystem.hpp;flatset/flatset.hpp;yamlParser/yamlParser.hpp")

DEAL_II_SETUP_TARGET("${PROJECT_NAME}")
DEAL_II_SETUP_TARGET(cpackexamplelib)



include(GNUInstallDirs)
install(TARGETS "${PROJECT_NAME}" cpackexamplelib
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/include/cpackexamplelib
  INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/helloworld
  )

