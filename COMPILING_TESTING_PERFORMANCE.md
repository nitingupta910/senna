COMPILING
=========

## Intel MKL

First, Intel MKL must be installed, and the following variables must be defined in the file `/etc/profile.d/intel.sh`:
```shell
# setting up Intel environment vars
INTELROOT="/opt/intel"
MKLROOT="${INTELROOT}/mkl"

# setting up MKL environment for sh
. ${MKLROOT}/bin/mklvars.sh intel64 lp64
```

Command to compile:
```shell
gcc -o senna -O3 -ffast-math *.c -DUSE_MKL_BLAS -I${INTELROOT}/compilers_and_libraries/linux/mkl/include -L${MKLROOT}/lib/intel64 -Wl,--no-as-needed -lm -lmkl_intel_lp64 -lmkl_sequential -lmkl_core -pthread
```
or
```shell
gcc -o senna -O3 -ffast-math *.c -DUSE_MKL_BLAS -I/opt/intel/compilers_and_libraries/linux/mkl/include -L/opt/intel/mkl/lib/intel64 -Wl,--no-as-needed -lm -lmkl_intel_lp64 -lmkl_sequential -lmkl_core -pthread
```

## ATLAS
First, it is necessary to set ATLAS with:
```shell
sudo update-alternatives --config libblas.so.3 ## select from the version of blas: atlas
sudo update-alternatives --config liblapack.so.3 ## select from the version of lapack: atlas
```
And then compile with:
```shell
gcc -o senna -O3 -ffast-math *.c -DUSE_ATLAS_BLAS /usr/lib/atlas-base/atlas/*
```

## Simple
```shell
gcc -o senna -O3 -ffast-math *.c
```


TESTING
=======
And to test the compiled file (regardless of the forms above), just run the command below:
```shell
time ./senna < sanity-test-input.txt > sanity-test-result.txt
```
The resulting file is named `sanity-test-result.txt`, and its contents must be the same as the existing file named `sanity-test-output.txt`.


PERFORMANCE
===========
The three forms have been tested, and the first form (__Intel MKL__) is the fastest. Here are the runtimes:

__Intel MKL__
```shell
real	1m11.372s
user	1m11.264s
sys	0m0.100s
```

__ATLAS__
```shell
real	2m3.740s
user	1m49.252s
sys	0m19.484s
```

__Simple with OpenBlas__
```shell
real	2m17.415s
user	2m17.208s
sys	0m0.196s
```

__Simple with Blas__
```shell
real	2m19.058s
user	2m18.968s
sys	0m0.076s
```
