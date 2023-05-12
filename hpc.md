## 
 ``` bash
 
    mkdir hpc_dir
    cd hpc_dir/
    sudo apt-get install build-essential
    sudo apt-get install libopenmpi-dev
    sudo apt-get install libblas-dev liblapack-dev libfftw3-dev
    sudo apt-get install vim
    sudo apt-get install git
    sudo apt-get update
    
    sudo apt-get install build-essential
    gcc --version
    vim hello.c
```    
``` c
#include <stdio.h>

int main() {
   printf("Hello, World!\n");
   return 0;
}

    
```    
``` bash

    gcc -o hello hello.c
    ./hello
```
``` bash
    sudo apt-get update
    sudo apt-get install openmpi-bin libopenmpi-dev
    mpirun --version
    vim mpi_hello.c
```
``` c
#include <stdio.h>
#include <mpi.h>

int main(int argc, char** argv) {
   int rank, size;
   MPI_Init(&argc, &argv);
   MPI_Comm_rank(MPI_COMM_WORLD, &rank);
   MPI_Comm_size(MPI_COMM_WORLD, &size);
   printf("Hello, world! I am %d of %d.\n", rank, size);
   MPI_Finalize();
   return 0;
}
```
``` bash   
    mpicc -o mpi_hello mpi_hello.c
    mpirun -np 4 ./mpi_hello
    
```
``` bash
    sudo apt-get update
    sudo apt-get install libomp-dev
    sudo vim omp_hello.c
```
``` c
#include <stdio.h>
#include <omp.h>

int main() {
   #pragma omp parallel
   {
      int tid = omp_get_thread_num();
      printf("Hello, world! I am thread %d.\n", tid);
   }
   return 0;
}

```
``` bash
    gcc -fopenmp -o omp_hello omp_hello.c
    ./omp_hello
    sudo apt-get update
```
``` bash
   sudo apt-get install ocl-icd-opencl-dev
   vim cl_hello.c
 ``` 
 ``` c
 
#include <stdio.h>
#include <CL/cl.h>

int main() {
   cl_uint num_platforms, num_devices;
   cl_platform_id platform;
   cl_device_id device;
   cl_int err;

   err = clGetPlatformIDs(1, &platform, &num_platforms);
   err = clGetDeviceIDs(platform, CL_DEVICE_TYPE_GPU, 1, &device, &num_devices);

   cl_context context = clCreateContext(NULL, 1, &device, NULL, NULL, &err);
   cl_command_queue queue = clCreateCommandQueue(context, device, 0, &err);

   const char* source = "__kernel void hello() { printf(\"Hello, world!\\n\"); }";
   cl_program program = clCreateProgramWithSource(context, 1, &source, NULL, &err);
   err = clBuildProgram(program, 1, &device, NULL, NULL, NULL);

   cl_kernel kernel = clCreateKernel(program, "hello", &err);
   err = clEnqueueTask(queue, kernel, 0, NULL, NULL);

   clReleaseKernel(kernel);
   clReleaseProgram(program);
   clReleaseCommandQueue(queue);
   clReleaseContext(context);
   return 0;
}

 ```
 ``` bash
    gcc -o cl_hello cl_hello.c -lOpenCL
    ./cl_hello
```
