PROJECT(vtkExodus2)

INCLUDE_REGULAR_EXPRESSION(".*")

INCLUDE(CheckIncludeFile)
CHECK_INCLUDE_FILE( malloc.h EX_HAVE_MALLOC_H )

SET(EXODUSII_VERSION_MAJOR "2")
SET(EXODUSII_VERSION_MINOR "0")
SET(EXODUSII_VERSION_PATCH "0")
SET(EXODUSII_VERSION "${EXODUSII_VERSION_MAJOR}.${EXODUSII_VERSION_MINOR}")
SET(EXODUSII_VERSION_FULL "${EXODUSII_VERSION}.${EXODUSII_VERSION_PATCH}")

SET(EXODUSII_BUILD_SHARED_LIBS "${BUILD_SHARED_LIBS}")

#-----------------------------------------------------------------------------
# Option to allow the user to disable compiler warnings
#-----------------------------------------------------------------------------
OPTION (EXODUSII_DISABLE_COMPILER_WARNINGS "Disable compiler warnings" ON)
MARK_AS_ADVANCED(EXODUSII_DISABLE_COMPILER_WARNINGS)

IF (EXODUSII_DISABLE_COMPILER_WARNINGS)
  # MSVC uses /w to suppress warnings.  It also complains if another
  # warning level is given, so remove it.
  IF (MSVC)
    SET (EXODUSII_WARNINGS_BLOCKED 1)
    STRING (REGEX REPLACE "(^| )([/-])W[0-9]( |$)" " "
        CMAKE_C_FLAGS "${CMAKE_C_FLAGS}")
    SET (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /w")
  ENDIF ()
  IF(WIN32)
    ADD_DEFINITIONS (-D_CRT_SECURE_NO_WARNINGS)
  ENDIF()
  # Borland uses -w- to suppress warnings.
  IF (BORLAND)
    SET (EXODUSII_WARNINGS_BLOCKED 1)
    SET (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -w-")
  ENDIF ()

  # Most compilers use -w to suppress warnings.
  IF (NOT EXODUSII_WARNINGS_BLOCKED)
    SET (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -w")
  ENDIF ()
ENDIF ()

CONFIGURE_FILE(
  ${CMAKE_CURRENT_SOURCE_DIR}/include/exodusII_cfg.h.in
  ${CMAKE_CURRENT_BINARY_DIR}/include/exodusII_cfg.h
  @ONLY IMMEDIATE)

INCLUDE_DIRECTORIES(
  "${CMAKE_CURRENT_SOURCE_DIR}/include"
  "${CMAKE_CURRENT_BINARY_DIR}/include"
)

SET(cbind_SRCS
  exclos.c
  excn2s.c
  ex_conv.c
  excopy.c
  excre.c
  exerr.c
  exgatm.c
  exgcns.c
  exgcon.c
  exgcor.c
  exgcset.c
  exgcss.c
  exgean.c
  exgatn.c
  exgeat.c
  exgatt.c
  exgebi.c
  exgelb.c
  exgblk.c
  exgelc.c
  exgconn.c
  exgem.c
  exgenm.c
  exgev.c
  exgvar.c
  exgevid.c
  exgevt.c
  exgvart.c
  exgfrm.c
  exggv.c
  exggvt.c
  exgids.c
  exginf.c
  exgini.c
  exginix.c
  exgmap.c
  exgmp.c
  exgnam.c
  exgnams.c
  exgnm.c
  exgnnm.c
  exgnp.c
  exgns.c
  exgnsd.c
  exgnsi.c
  exgnstt.c
  exgnsv.c
  exgnsvid.c
  exgnv.c
  exgnvid.c
  exgnvt.c
  exgnvv.c
  exgoea.c
  exgoatt.c
  exgotv.c
  exgpa.c
  exgp.c
  exgpem.c
  exgpn.c
  exgqa.c
  exgsnl.c
  exgsetp.c
  exgsp.c
  exgset.c
  exgss.c
  exgssc.c
  exgsetd.c
  exgssd.c
  exgssi.c
  exgssn.c
  exgsstt.c
  exgssv.c
  exgssvid.c
  exgtim.c
  exgtt.c
  exgvan.c
  exgvid.c
  exgvnm.c
  exgvp.c
  exgvtt.c
  exgvv.c
  exinq.c
  exopen.c
  exopts.c
  expatn.c
  expatt.c
  expcab.c
  expclb.c
  expcns.c
  expcon.c
  expconn.c
  expcor.c
  expcset.c
  expcss.c
  expean.c
  expeat.c
  expelb.c
  expblk.c
  expelc.c
  expem.c
  expenm.c
  expev.c
  expfrm.c
  expgv.c
  expinf.c
  expini.c
  expinix.c
  expmap.c
  expmp.c
  expnam.c
  expnams.c
  expnm.c
  expnmap.c
  exgnmap.c
  expnnm.c
  expnp.c
  expns.c
  expnsd.c
  expnstt.c
  expnsv.c
  expnv.c
  expnvv.c
  expoea.c
  expoatt.c
  exppa.c
  expp.c
  exppem.c
  exppn.c
  expqa.c
  expsetp.c
  expset.c
  expsetd.c
  expsp.c
  expss.c
  expssd.c
  expsstt.c
  expssv.c
  exptim.c
  exptt.c
  expvan.c
  expvar.c
  expvnm.c
  expvpa.c
  expvpax.c
  expvp.c
  expvpc.c
  expvtt.c
  expvv.c
  exupda.c
  ex_utils.c
  exgattp.c
  exgidm.c
  exgvarnam.c
  exgvarnams.c
  exgvarparam.c
  exgvartab.c
  expattp.c
  expidm.c
  expvarnam.c
  expvarnams.c
  expvarparam.c
  expvartab.c
  exgcssc.c 
  exgecpp.c 
  exgnconn.c 
  exgncor.c 
  exgnnv.c 
  exgnoatt.c 
  exgnvar.c 
  expecpp.c 
  expncor.c 
  expnnv.c 
  expnoatt.c 
  expnvar.c 
  exppsetd.c
)


IF(APPLE)
  SET_SOURCE_FILES_PROPERTIES(
    ex_utils.c
    PROPERTIES
    COMPILE_FLAGS -fno-common)
ENDIF()


VTK_ADD_LIBRARY(vtkexoIIc ${cbind_SRCS})
TARGET_LINK_LIBRARIES(vtkexoIIc ${vtknetcdf_LIBRARIES})

#INCLUDE (${CMAKE_ROOT}/Modules/Dart.cmake)

# Turned off until ExoIICTests.cxx problem is fixed.
#IF (BUILD_TESTING)
#  ENABLE_TESTING()
#  ADD_SUBDIRECTORY(test)
#ENDIF ()

