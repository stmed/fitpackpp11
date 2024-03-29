cmake_minimum_required(VERSION 3.0.0)

# FITPACK FORTRAN code
enable_language(Fortran)

if(${CMAKE_Fortran_COMPILER} MATCHES "ifort.*")
    set(CMAKE_Fortran_FLAGS_RELEASE "${CMAKE_Fortran_FLAGS} -fltconsistency -O3")
    set(CMAKE_Fortran_FLAGS_DEBUG   "${CMAKE_Fortran_FLAGS} -fltconsistency")
endif()

set(fitpack_SRC
    fitpack/bispev.f
    fitpack/curfit.f
    fitpack/fpback.f
    fitpack/fpbisp.f
    fitpack/fpchec.f
    fitpack/fpcurf.f
    fitpack/fpbspl.f
    fitpack/fpdisc.f
    fitpack/fpgivs.f
    fitpack/fpknot.f
    fitpack/fporde.f
    fitpack/fprank.f
    fitpack/fprati.f
    fitpack/fprota.f
    fitpack/fpsurf.f
    fitpack/parcur.f
    fitpack/parder.f
    fitpack/fppara.f
    fitpack/splder.f
    fitpack/splev.f
    fitpack/surfit.f
)

add_library(fitpack STATIC 
            ${fitpack_SRC}
)

enable_language(CXX)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3")

set(GCC_COMPILE_FLAGS "-fpic -ffast-math")

add_definitions(${GCC_COMPILE_FLAGS})

# C++ interface
include(FortranCInterface)
FortranCInterface_HEADER(FCMangle.h
                         MACRO_NAMESPACE "FC_"
                         SYMBOL_NAMESPACE ""
                         SYMBOLS parcur curfit splev splder surfit bispev parder)
FortranCInterface_VERIFY(CXX)

# install
install(TARGETS fitpack DESTINATION lib)
install(FILES "${PROJECT_BINARY_DIR}/FCMangle.h" DESTINATION include/fitpack)