# QVina-W-GPU
QVina-W-GPU is proposed to parallelize the efficient QVina-W docking software, which can exploit the highly-parallel hardware architecture to speed up the process of molecule docking.

## Compiling and Running 

**Note**: at least one GPU card is required and make sure the version of GPU driver is up to date

## Windows

**Run on the executable file**

1. For the first time to use QVina-GPU, please run `QVina-W-GPU-K.exe` with command `./Qvina-W-GPU-K.exe --config ./input_file_example/2bm2_config.txt`
   You are supposed to have the docking results `2bm2_out.pdbqt` of our example complex and a `Kernel2_Opt.bin` file

2. Once you have the `Kernel2_Opt.bin` file, you can run `Qvina-W-GPU.exe` without compiling the kernel files (thus to save more runtime)

   When you run `Qvina-W-GPU.exe`, please make sure `Kernel2_Opt.bin` file are in the same directory

#### Build from source file

>Visual Studio 2019 is recommended for build Vina-GPU from source

1. install [boost library](https://www.boost.org/) (current version is 1.77.0)

2. install [CUDA Toolkit](https://developer.nvidia.com/zh-cn/cuda-toolkit) (current version is v11.5) if you are using NVIDIA GPU cards

   Note: the OpenCL library can be found in CUDA installation path for NVIDIA or in the driver installation path for AMD

3. add `./lib` `./OpenCL/inc` `$(YOUR_BOOST_LIBRARY_PATH)/boost` `$(YOUR_CUDA_TOOLKIT_LIBRARY_PATH)/CUDA/v11.5/include` in the include directories

4. add `$(YOUR_BOOST_LIBRARY_PATH)/stage/lib` `$(YOUR_CUDA_TOOLKIT_PATH)/CUDA/lib/Win32`in the addtional library 

5. add `OpenCL.lib` in the additional dependencies 

6. add `--config ./input_file_example/2bm2_config.txt` in the command arguments

7. add `WIN32` in the preprocessor definitions if necessary

8. if you want to compile the binary kernel file on the fly, add `BUILD_KERNEL_FROM_SOURCE` in the preprocessor definitions

9. build & run

Note: ensure the line ending are CLRF

### Linux

**Note**: At least 8M stack size is needed. To change the stack size, use `ulimit -s 8192`.

1. install [boost library](https://www.boost.org/) (current version is 1.77.0)

2. install [CUDA Toolkit](https://developer.nvidia.com/zh-cn/cuda-toolkit) (current version is 11.5) if you are using NVIDIA GPU cards

   note: OpenCL library can be usually in `/usr/local/cuda` (for NVIDIA GPU cards)

3. change the `BOOST_LIB_PATH` and `OPENCL_LIB_PATH` accordingly in `Makefile`

4. set GPU platform `GPU_PLATFORM` and OpenCL version `OPENCL_VERSION`in `Makefile`. some options are given below:

   **Note**: -DOPENCL_3_0 is highly recommended in Linux. To check the OpenCL version on a given platform, use `clinfo`.

|Macros|Options|Descriptions|
|--|--|--|	
|GPU_PLATFORM|-DNVIDIA_PLATFORM / -DAMD_PLATFORM|NVIDIA / AMD GPU platform
|  OPENCL_VERSION | -DOPENCL_3_0 / -DOPENCL_1_2|OpenCL version 3.0 / 1.2

6. type `make clean` and `make source` to build Vina-GPU that compile the kernel files on the fly (this would take some time at the first use)
7. after a successful compiling, `Vina-GPU` can be seen in the directory 
8. type `./Qvina-W-GPU --config ./input_file_example/2bm2_config.txt` to run QVina-W-GPU
9. once you successfully run QVina-W-GPU, its runtime can be further reduced by typing `make clean` and `make` to build it without compiling kernel files (but make sure the `Kernel2_Opt.bin` file is **unchanged**)

10. other compile options: 

| Options                 | Description                |
| ----------------------- | -------------------------- |
| -g                      | debug                      |
| -DDISPLAY_ADDITION_INFO | print addition information |