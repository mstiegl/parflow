
# TODO many little tools not included yet.  See Makefile.in

set (TOOLS_SRC_FILES pftappinit.c printdatabox.c readdatabox.c databox.c
  error.c velocity.c head.c flux.c diff.c stats.c tools_io.c axpy.c
  getsubbox.c enlargebox.c load.c usergrid.c grid.c region.c file.c
  pftools.c top.c compute_domain.c water_balance.c water_table.c
  toposlopes.c sum.c
  )

add_library(pftools SHARED ${TOOLS_SRC_FILES})

target_include_directories(pftools PUBLIC ${TCL_INCLUDE_PATH})
target_link_libraries(pftools ${TCL_LIBRARY})

if (${PARFLOW_HAVE_SILO})
  target_include_directories (pftools PUBLIC "${SILO_INCLUDE_DIRS}")
  target_link_libraries (pftools ${SILO_LIBRARIES})
endif (${PARFLOW_HAVE_SILO})

if (${PARFLOW_HAVE_HDF5})
  target_include_directories (pftools PUBLIC "${HDF5_INCLUDE_DIRS}")
  target_link_libraries (pftools ${HDF5_LIBRARIES})
endif (${PARFLOW_HAVE_HDF5})

if (${PARFLOW_HAVE_ZLIB})
  target_link_libraries (pftools ${ZLIB_LIBRARIES})
endif (${PARFLOW_HAVE_ZLIB})

if (${PARFLOW_HAVE_SZLIB})
  target_link_libraries (pftools ${SZLIB_LIBRARIES})
endif (${PARFLOW_HAVE_SZLIB})

install(TARGETS pftools DESTINATION bin)

file (GLOB TCL_SRC *.tcl)
install(FILES ${TCL_SRC} DESTINATION bin)

set(SCRIPTS pfhelp pfmvio pfbtosa pfbtovis pfsbtosa pfstrip)
install(FILES ${SCRIPTS} DESTINATION bin)

add_executable(pfwell_cat pfwell_cat.c well.c)
install(TARGETS pfwell_cat DESTINATION bin)

add_executable(pfwell_data pfwell_data.c well.c)
install(TARGETS pfwell_data DESTINATION bin)

add_executable(gmssol2pfsol gmsSOL2pfsol.c gmsreadSOL.c geom_sort_vertices.c)
install(TARGETS gmssol2pfsol DESTINATION bin)

add_executable(gmstinvertices gmsTINvertices.c gmsreadTIN.c gmswriteTIN.c geom_sort_xy_vertices.c)
install(TARGETS gmstinvertices DESTINATION bin)

add_executable(gmstriangulate gmstriangulate.c gmsreadTIN.c gmswriteTIN.c geom_sort_xy_vertices.c)
install(TARGETS gmstriangulate DESTINATION bin)

add_executable(projecttin projecttin.c gmsreadTIN.c)
target_link_libraries(projecttin ${PARFLOW_LIBM})
install(TARGETS projecttin DESTINATION bin)

