# Prerequisites

    cmake --version
    locate libopenmpi-dev
    locate openmpi-bin
    locate libfftw3-dev
# What we are going to do?

- Install Intel oneMKL
- Install Intel Basekit
- Install OpenMPI(Locally in a build directory)
- Install wannier90
- Install ScaLAPACK (We have scalapack in intel mkl but a separate one is good)
- Install VASP(Locally)

# Download all tar files at the location INSTALL

1. Install Intel mkl

[Download Offline Installer](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl-download.html?operatingsystem=linux&linux-install=offline)
# Just give an email and download
# In our case the filename is "l_onemkl_p_2024.2.1.105_offline.sh"
# To Install just execute 
    sudo sh ./l_onemkl_p_2024.2.0.664_offline.sh

2. Install Intel Basekit

[Download Offline Installer](https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?operatingsystem=linux&linux-install-type=offline)
# Just give an email and download
# In our case the filename is "l_BaseKit_p_2024.2.1.100_offline.sh"
# To Install just execute 
    sudo sh ./l_BaseKit_p_2024.2.0.634_offline.sh -a --silent --cli --eula accept

# To configure these libraries open a terminal and execute
    sudo ldconfig
# Local activate
    export PATH=/opt/intel/oneapi/mkl/2024.2/bin:$PATH
    export LD_LIBRARY_PATH=/opt/intel/oneapi/mkl/2024.2/lib:$LD_LIBRARY_PATH
    
3. Install OpenMPI 5 (openmpi-5.0.5)

[Download openmpi-5.0.5](https://download.open-mpi.org/release/open-mpi/v5.0/openmpi-5.0.5.tar.gz)
# Download and extract the files

    cd openmpi-5.0.5
    mkdir build
    cd build
    pwd
# gives
    /home/vincent/INSTALL/openmpi-5.0.5/build
# copy 
    cd ..
    ./configure --prefix=/home/vincent/INSTALL/openmpi-5.0.5/build
    make install
# Local activate
    export PATH=/home/vincent/INSTALL/openmpi-5.0.5/build/bin:$PATH
    export LD_LIBRARY_PATH=/home/vincent/INSTALL/openmpi-5.0.5/build/lib:$LD_LIBRARY_PATH

4. Install Wannier90
# Make wannier with the added makefile "make.inc"
[Download Wannier v3.1.0](https://github.com/wannier-developers/wannier90/archive/v3.1.0.tar.gz)
# Download and extract the file
    cd wannier90-3.1.0
    make
    make install
    make tests 
# Some tests may fail we dont't know why...!
5. Install ScaLAPACK(scalapack-2.2.0)
[Download](https://github.com/Reference-ScaLAPACK/scalapack/archive/refs/tags/v2.2.0.tar.gz)
# Download and extract the file
# Download either the "SLmake.inc" or rename "SLmake.inc.example" to "SLmake.inc" with following changes
    FC            = mpif90 -fallow-argument-mismatch
    FCFLAGS       = -O3 -std=legacy
# Now we are ready to Install VASP

7. Install VASP(vasp.6.4.0)
    cd vasp.6.4.0
# We have included the "makefile.include" in this repository, and give necessary changes(if needed) and just go to make
    make DEPS=1 -j
# If you need to remake VASP
    make veryclean
    make DEPS=1 -j
# SET ENV
    export PATH=/home/vincent/INSTALL/openmpi-5.0.5/build/bin:$PATH
    export LD_LIBRARY_PATH=/home/vincent/INSTALL/openmpi-5.0.5/build/lib:$LD_LIBRARY_PATH
    export PATH=/opt/intel/oneapi/mkl/2024.2/bin:$PATH
    export LD_LIBRARY_PATH=/opt/intel/oneapi/mkl/2024.2/lib:$LD_LIBRARY_PATH
# Running VASP executable(Pure MPI)
    mpirun -np 4 --map-by node:PE=1 --bind-to core -x OMP_NUM_THREADS=1 -x OMP_STACKSIZE=512m -x OMP_PLACES=cores -x OMP_PROC_BIND=close /home/vincent/INSTALL/vasp.6.4.0/bin/vasp_std > output &
# Following are some additional useful commands
# to check no: of cores
    cat /proc/cpuinfo | grep "cpu cores" | uniq
## In our case
    8
# to check no: of logical cores
    cat /proc/cpuinfo | grep "processor" | wc -l
## In our case
    16
# This means that we have a total core 16, but in terms of 8 $\times$ 2
# That is maximum value of $ncores = 8 in mpirun
    mpirun -np $ncores
# How to run vasp at a specific location
# Go to that location and open a terminal and execute
    export PATH=/home/vincent/INSTALL/openmpi-5.0.5/build/bin:$PATH
    export LD_LIBRARY_PATH=/home/vincent/INSTALL/openmpi-5.0.5/build/lib:$LD_LIBRARY_PATH
    export PATH=/opt/intel/oneapi/mkl/2024.2/bin:$PATH
    export LD_LIBRARY_PATH=/opt/intel/oneapi/mkl/2024.2/lib:$LD_LIBRARY_PATH
# And then,
    mpirun -np 4 --map-by node:PE=1 --bind-to core -x OMP_NUM_THREADS=1 -x OMP_STACKSIZE=512m -x OMP_PLACES=cores -x OMP_PROC_BIND=close /home/vincent/INSTALL/vasp.6.4.0/bin/vasp_std > output &
# This command is adapted from VASP "testsuite" -> "ompi+omp.conf"

    nranks=4
    nthrds=2
    mpirun -np $nranks --map-by node:PE=$nthrds --bind-to core -x OMP_NUM_THREADS=$nthrds -x OMP_STACKSIZE=512m -x OMP_PLACES=cores -x OMP_PROC_BIND=close vasp_executable
