cmake_minimum_required(VERSION 3.31)
set(CMAKE_PROJECT_TOP_LEVEL_INCLUDES .devcontainer/cmake/xproinc.cmake)
project(rapidxml)
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
if(DEFINED XP_NAMESPACE)
  xpExternPackage(NAMESPACE ${XP_NAMESPACE}
    TARGETS_FILE ${targetsFile} LIBRARIES ${lib_name}
    BASE v1.13 XPDIFF "intro"
    WEB "http://rapidxml.sourceforge.net/"
    DESC "fast XML parser"
    LICENSE "[BSL-1.0 OR MIT](http://rapidxml.sourceforge.net/license.txt 'Boost Software License or MIT License')"
    )
  set(nameSpace NAMESPACE ${XP_NAMESPACE}::)
elseif(NOT DEFINED CMAKE_INSTALL_CMAKEDIR)
  set(CMAKE_INSTALL_CMAKEDIR ${CMAKE_INSTALL_DATADIR}/cmake)
endif()
install(TARGETS ${lib_name} EXPORT ${targetsFile})
install(FILES ${root_srcs} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})
install(EXPORT ${targetsFile} DESTINATION ${CMAKE_INSTALL_CMAKEDIR} ${nameSpace})
