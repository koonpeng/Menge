cmake_minimum_required(VERSION 2.8)

project(MENGE_CORE)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${MENGE_EXE_DIR})

file(
	GLOB_RECURSE
	source_files
	${MENGE_SRC_DIR}/MengeCore/*.cpp
  ${MENGE_SRC_DIR}/MengeCore/*.h
)


add_library(
	mengeCore
	SHARED
	${database_files}
	${source_files}
)

target_link_libraries ( mengeCore PUBLIC dl PRIVATE tinyxml )

install( TARGETS mengeCore DESTINATION ${LIBRARY_OUTPUT_PATH} )

if (UNIX AND NOT APPLE)
  include(GNUInstallDirs)
  install(TARGETS mengeCore
    EXPORT Menge
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/Menge
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/Menge
  )
  install(
    DIRECTORY
      ${MENGE_SRC_DIR}/MengeCore
      ${MENGE_SRC_DIR}/../include/
      ${MENGE_SRC_DIR}/tinyxml
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/Menge
    FILES_MATCHING PATTERN *.h
  )
  install(EXPORT Menge
    NAMESPACE Menge::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/Menge/cmake
    FILE MengeConfig.cmake
  )
endif()
