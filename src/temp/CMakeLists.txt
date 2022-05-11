cmake_minimum_required(VERSION 3.0)

#set(CMAKE_VERBOSE_MAKEFILE ON)
#set(CMAKE_RULE_MESSAGES ON)

set(CMAKE_CONFIGURATION_TYPES Debug Optim CACHE TYPE INTERNAL FORCE)
set(CMAKE_BUILD_TYPE "optim" CACHE STRING "Build Type: Debug or Optim")
set(ENABLE_FFTW OFF CACHE BOOL "Enable the FFTW FFT processor (GPL)")
set(ENABLE_NAYUKI_PORTABLE OFF CACHE BOOL "Enable the Nayuki portable FFT processor (MIT)")
set(ENABLE_NAYUKI_AVX OFF CACHE BOOL "Enable the Nayuki AVX assembly FFT processor (MIT)")
set(ENABLE_SPQLIOS_AVX OFF CACHE BOOL "Enable the SPQLIOS AVX assembly FFT processor")
set(ENABLE_SPQLIOS_FMA OFF CACHE BOOL "Enable the SPQLIOS FMA assembly FFT processor")
set(ENABLE_TESTS OFF CACHE BOOL "Build the tests (requires googletest)")

project(tfhe)

if ("${CMAKE_CXX_COMPILER_ID}" MATCHES "AppleClang")
  # https://stackoverflow.com/a/16229679
  set(CLANG_FLAGS "-stdlib=libc++")
endif()

# -std=c99 seems to be required in Travis tests for whatever reason
set(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -std=c99")

set(CMAKE_CXX_FLAGS_DEBUG "${CLANG_FLAGS} -std=gnu++11 -g3 -O0 -Wall -Werror")
set(CMAKE_C_FLAGS_DEBUG "-g3 -O0 -Wall -Werror")

set(CMAKE_CXX_FLAGS_OPTIM "${CLANG_FLAGS} -std=gnu++11 -g3 -march=native -O2 -DNDEBUG -funroll-loops -Wall -Werror")
set(CMAKE_C_FLAGS_OPTIM "-g3 -march=native -O3 -DNDEBUG -funroll-loops -Wall -Werror")

set(CMAKE_CXX_FLAGS_RELEASE "${CLANG_FLAGS} -std=gnu++11 -g0 -march=native -O2 -DNDEBUG -funroll-loops -Wall -Werror")
set(CMAKE_C_FLAGS_RELEASE "-g0 -march=native -O3 -DNDEBUG -funroll-loops -Wall -Werror")

# Runtime path setup
set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}")
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

# Source Tree
set(TFHE_INCLUDES_DIR ${CMAKE_CURRENT_LIST_DIR}/src/)



# Install
set(TFHE_CONFIG_INSTALL_DIR ${CMAKE_INSTALL_LIBDIR}/cmake/TFHE)
set(TFHE_INCLUDES_INSTALL_DIR ${CMAKE_INSTALL_INCLUDEDIR}/TFHE)

set(TFHE_PKGCONFIG_INSTALL_DIR ${CMAKE_INSTALL_LIBDIR}/pkgconfig)



include_directories("include")
file(GLOB TFHE_HEADERS include/*.h)

install(FILES ${TFHE_HEADERS}
    DESTINATION include/tfhe
    PERMISSIONS OWNER_READ OWNER_WRITE GROUP_READ WORLD_READ
)

add_subdirectory(libtfhe)