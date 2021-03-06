include_directories(${PROJECT_SOURCE_DIR}/src)
include_directories(${PROJECT_BINARY_DIR}/src)

#------------------------------------------------------------------------------
# Include CPP files to add to the library target
#
# each subdir can contain a ModuleSRC.txt file 
# with a set command on the variable ${DGTAL_SRC}
# 
#------------------------------------------------------------------------------
include(DGtal/arithmetic/ModuleSRC.txt)
include(DGtal/kernel/ModuleSRC.txt)
include(DGtal/base/ModuleSRC.txt)
include(DGtal/io/ModuleSRC.txt)
include(DGtal/math/ModuleSRC.txt)
## Board dependency
include(Board/ModuleSRC.txt)


#------------------------------------------------------------------------------
# DGtal Target
#------------------------------------------------------------------------------
IF (BUILD_SHARED_LIBS)
  add_library (DGtal SHARED ${DGTAL_SRC} ${BOARD_SRC} )
  target_link_libraries( DGtal ${DGtalLibDependencies} )
ELSE(BUILD_SHARED_LIBS)
  add_library (DGtal STATIC ${DGTAL_SRC} ${BOARD_SRC})
  target_link_libraries(DGtal ${DGtalLibDependencies} )
ENDIF(BUILD_SHARED_LIBS)

install(TARGETS DGtal
  # IMPORTANT: Add the DGtal library to the "export-set"
  EXPORT DGtalLibraryDepends
  RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin
  LIBRARY DESTINATION "${INSTALL_LIB_DIR}" COMPONENT shlib
  ARCHIVE DESTINATION "${INSTALL_LIB_DIR}"
 #PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}/DGtal"
  COMPONENT dev)

#------------------------------------------------------------------------------
# DGtalIO Target
#------------------------------------------------------------------------------
IF (BUILD_SHARED_LIBS)
  add_library ( DGtalIO SHARED ${DGTALIO_SRC})
  target_link_libraries( DGtalIO DGtal ${DGtalLibDependencies} )
ELSE(BUILD_SHARED_LIBS)
  add_library ( DGtalIO STATIC ${DGTALIO_SRC})
  target_link_libraries( DGtalIO DGtal ${DGtalLibDependencies} )
ENDIF(BUILD_SHARED_LIBS)

install(TARGETS DGtalIO
  # IMPORTANT: Add the DGtalIO library to the "export-set"
  EXPORT DGtalLibraryDepends
  RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin
  LIBRARY DESTINATION "${INSTALL_LIB_DIR}" COMPONENT shlib
  ARCHIVE DESTINATION "${INSTALL_LIB_DIR}"
  # PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}/DGtal"
  COMPONENT dev)

#------------------------------------------------------------------------------
# Misc
#------------------------------------------------------------------------------
IF( CMAKE_SYSTEM_PROCESSOR STREQUAL "x86_64" )
  SET_TARGET_PROPERTIES(DGtal PROPERTIES COMPILE_FLAGS "-fPIC")
#  SET_TARGET_PROPERTIES(DGtalIO PROPERTIES COMPILE_FLAGS "-fPIC")
ENDIF( CMAKE_SYSTEM_PROCESSOR STREQUAL "x86_64" )
