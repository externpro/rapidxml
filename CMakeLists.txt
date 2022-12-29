cmake_minimum_required(VERSION 3.24)
project(rapidxml)
include(GNUInstallDirs)
include(flags OPTIONAL)
set(lib_name ${PROJECT_NAME})
########################################
set(root_srcs
  license.txt
  manual.html
  rapidxml.hpp
  rapidxml_iterators.hpp
  rapidxml_print.hpp
  rapidxml_utils.hpp
  )
source_group("" FILES ${root_srcs})
list(APPEND ${lib_name}_libsrcs ${root_srcs})
########################################
add_library(${lib_name} INTERFACE ${${lib_name}_libsrcs})
target_include_directories(${lib_name} INTERFACE $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
########################################
set(targetsFile ${PROJECT_NAME}-targets)
install(TARGETS ${lib_name} EXPORT ${targetsFile})
install(FILES ${root_srcs} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})
if(DEFINED XP_NAMESPACE)
  set(nameSpace NAMESPACE ${XP_NAMESPACE}::)
endif()
if(NOT DEFINED XP_INSTALL_CMAKEDIR)
  set(XP_INSTALL_CMAKEDIR ${CMAKE_INSTALL_DATADIR}/cmake)
endif()
install(EXPORT ${targetsFile} DESTINATION ${XP_INSTALL_CMAKEDIR} ${nameSpace})
