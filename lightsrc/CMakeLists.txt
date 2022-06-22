cmake_minimum_required(VERSION 3.0)

#set(CMAKE_VERBOSE_MAKEFILE ON)
#set(CMAKE_RULE_MESSAGES ON)

set(CMAKE_CONFIGURATION_TYPES Debug Optim CACHE TYPE INTERNAL FORCE)
set(CMAKE_BUILD_TYPE "optim" CACHE STRING "Build Type: Debug or Optim")

project(tfhe)

# -std=c99 seems to be required in Travis tests for whatever reason
set(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -std=c99")

set(CMAKE_CXX_FLAGS_DEBUG "${CLANG_FLAGS} -std=gnu++11 -g3 -O0 -Wall -Werror -nostdinc -fvisibility=hidden -fpie -fstack-protector -I/opt/intel/sgxsdk/include -I/opt/intel/sgxsdk/include/libcxx -I/opt/intel/sgxsdk/include/tlibc -nostdinc++")
set(CMAKE_C_FLAGS_DEBUG "-g3 -O0 -Wall -Werror -nostdinc -fvisibility=hidden -fpie -fstack-protector -I/opt/intel/sgxsdk/include -I/opt/intel/sgxsdk/include/libcxx -I/opt/intel/sgxsdk/include/tlibc")

set(CMAKE_CXX_FLAGS_OPTIM "${CLANG_FLAGS} -std=gnu++11 -g3 -march=native -O2 -DNDEBUG -funroll-loops -Wall -Werror -nostdinc -fvisibility=hidden -fpie -fstack-protector -I/opt/intel/sgxsdk/include -I/opt/intel/sgxsdk/include/libcxx -I/opt/intel/sgxsdk/include/tlibc -nostdinc++")
set(CMAKE_C_FLAGS_OPTIM "-g3 -march=native -O3 -DNDEBUG -funroll-loops -Wall -Werror -nostdinc -fvisibility=hidden -fpie -fstack-protector -I/opt/intel/sgxsdk/include -I/opt/intel/sgxsdk/include/libcxx -I/opt/intel/sgxsdk/include/tlibc")

set(CMAKE_CXX_FLAGS_RELEASE "${CLANG_FLAGS} -std=gnu++11 -g0 -march=native -O2 -DNDEBUG -funroll-loops -Wall -Werror -nostdinc -fvisibility=hidden -fpie -fstack-protector -I/opt/intel/sgxsdk/include -I/opt/intel/sgxsdk/include/libcxx -I/opt/intel/sgxsdk/include/tlibc -nostdinc++")
set(CMAKE_C_FLAGS_RELEASE "-g0 -march=native -O3 -DNDEBUG -funroll-loops -Wall -Werror -nostdinc -fvisibility=hidden -fpie -fstack-protector -I/opt/intel/sgxsdk/include -I/opt/intel/sgxsdk/include/libcxx -I/opt/intel/sgxsdk/include/tlibc")

set(CMAKE_INSTALL_OLDINCLUDEDIR "/opt/intel/sgxsdk/include")


include_directories("include")
file(GLOB TFHE_HEADERS include/*.h)

set(CMAKE_INSTALL_PREFIX "~/mytfhe")

install(FILES ${TFHE_HEADERS}
    DESTINATION ${CMAKE_INSTALL_PREFIX}/include/tfhe
    PERMISSIONS OWNER_READ OWNER_WRITE GROUP_READ WORLD_READ)
install(FILES build/libtfhe/libtfhe.a
    DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
    PERMISSIONS OWNER_READ OWNER_WRITE GROUP_READ WORLD_READ)

# include the lib and the tests
add_subdirectory(libtfhe)
