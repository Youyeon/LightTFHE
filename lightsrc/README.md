# Build Light-TFHE static library for SGX
## Prerequisite
SGX-SDK is installed in your machine (path: `/opt/intel/sgxsdk`)

- to build
```
    cmake -S . -B build
    cmake --build build
    sudo cmake --install build
```
- then you can see tfhe header files & static library ${home}/mytfhe_sgx
