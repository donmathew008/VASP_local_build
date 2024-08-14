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
5. Install 
6. Install VASP}}$  
    cd ../../
    mkdir VASP
    cp vasp.6.4.0_0.tgz VASP
    cd VASP
    tar xvzf vasp.6.4.0_0.tgz
    cd vasp.6.4.0
    cd arch
    cp makefile.include.aocc_ompi_aocl_omp ../makefile.include # skip this step if using our makefile

# We have included the "makefile.include.aocc_aocl_ompi" in this repository, download and rename it as "makefile.include" and give necessary changes and just go to make
    cd ..
    make DEPS=1 -j
# If you need to remake VASP
    make veryclean
    make DEPS=1 -j
# SET ENV
    source /home/vincent/INSTALL/VASP_AMD/AOCC/setenv_AOCC.sh
    source /home/vincent/INSTALL/VASP_AMD/AOCL/build/4.2.0/aocc/amd-libs.cfg
    export PATH=/home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build/bin:$PATH
    export LD_LIBRARY_PATH=/home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build/lib:$LD_LIBRARY_PATH
    export PATH=/home/vincent/INSTALL/VASP_AMD/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build/bin:$PATH
    export LD_LIBRARY_PATH=/home/vincent/INSTALL/VASP_AMD/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build/lib:$LD_LIBRARY_PATH
    mpirun -np 1 /home/vincent/INSTALL/VASP_GCC/vasp6/vasp.6.4.0/bin/vasp_ncl > output &
# Path of AOCL
    /home/vincent/INSTALL/VASP_AMD/AOCL/build/4.2.0/aocc
# Following are some additional useful commands
# to check no: of cores
    cat /proc/cpuinfo | grep "cpu cores" | uniq
## In our case
    4
# to check no: of logical cores
    cat /proc/cpuinfo | grep "processor" | wc -l
## In our case
    8
# This means that we have a total core 8, but in terms of 4 $\times$ 2
# That is maximum value of $ncores = 4 in mpirun
    mpirun -np $ncores
# Final running command for pure mpi parallelization (OMP_NUM_THREADS=1)
    mpirun -np 4 --map-by node:PE=1 --bind-to core -x OMP_NUM_THREADS=1 -x OMP_STACKSIZE=512m -x OMP_PLACES=cores -x OMP_PROC_BIND=close /home/vincent/INSTALL/VASP_gcc_omp/vasp.6.4.0/bin/vasp_std > output &

