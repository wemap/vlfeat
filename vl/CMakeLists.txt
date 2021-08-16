set(VLFEAT_SRC
    aib.c
    array.c
    covdet.c
    dsift.c
    fisher.c
    generic.c
    getopt_long.c
    gmm.c
    hikmeans.c
    hog.c
    homkermap.c
    host.c
    ikmeans.c
    imopv_sse2.c
    imopv.c
    kdtree.c
    kmeans.c
    lbp.c
    liop.c
    mathop_avx.c
    mathop_sse2.c
    mathop.c
    mser.c
    pgm.c
    quickshift.c
    random.c
    rodrigues.c
    scalespace.c
    sift.c
    slic.c
    stringop.c
    svm.c
    svmdataset.c
    vlad.c
)

set(VLFEAT_HEADERS
    aib.h
    array.h
    covdet.h
    dsift.h
    fisher.h
    generic.h
    getopt_long.h
    gmm.h
    heap-def.h
    hikmeans.h
    hog.h
    homkermap.h
    host.h
    ikmeans.h
    imopv_sse2.h
    imopv.h
    kdtree.h
    kmeans.h
    lbp.h
    liop.h
    mathop_avx.h
    mathop_sse2.h
    mathop.h
    mser.h
    pgm.h
    qsort-def.h
    quickshift.h
    random.h
    rodrigues.h
    scalespace.h
    shuffle-def.h
    sift.h
    slic.h
    stringop.h
    svm.h
    svmdataset.h
    vlad.h
)

set (VLFEAT_HEADERS ${VLFEAT_HEADERS} PARENT_SCOPE)

add_library(vlfeat ${VLFEAT_SRC})

# Handle `#include <vlfeat/foo.h>`.
target_include_directories(vlfeat PUBLIC
  $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}>)

# Link OpenMP_C
find_package(OpenMP REQUIRED)
message(STATUS "Use OpenMP ${OpenMP_VERSION}")
target_link_libraries(vlfeat PRIVATE OpenMP::OpenMP_C)

if (ENABLE_AVX)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mavx")
set(VL_DISABLE_AVX OFF)
else()
set(VL_DISABLE_AVX ON)
endif()

install(TARGETS vlfeat
  EXPORT vlfeat-targets
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)


foreach(header ${VLFEAT_HEADERS})
  get_filename_component(dir ${header} DIRECTORY )
  install(FILES ${header}
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/vlfeat/${dir}
  )
endforeach()

install(EXPORT vlfeat-targets
  DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/vlfeat
)
