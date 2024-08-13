### VASP installation trial with GNU and AOCL locally
[Ref](https://implant.fs.cvut.cz/)

#### Prerequisites

    cmake --version
    locate libopenmpi-dev
    locate openmpi-bin
    locate libfftw3-dev

#### Download all tar files at the location VASP_GCC
# $\color{Violet}{\textbf{Install AOCC}}$
    mkdir AOCC
    cp aocc-compiler-4.2.0.tar AOCC
    cd AOCC
    tar -xvf aocc-compiler-4.2.0.tar
    cd aocc-compiler-4.2.0/
    ./install.sh
    source /home/vincent/INSTALL/VASP_GCC/AOCC/setenv_AOCC.sh
    which clang
    clang -v
    ./AOCC-prerequisites-check.sh

# $\color{Violet}{\textbf{Install AOCL}}$

    cd ../../
    mkdir AOCL
    cp aocl-linux-aocc-4.2.0.tar.gz AOCL
    cd AOCL
    tar -xvf aocl-linux-aocc-4.2.0.tar.gz
    mkdir build
    cd build
    pwd
#### which gives
    /home/vincent/INSTALL/VASP_GCC/AOCL/build
#### copy path
    ./install.sh -t /home/vincent/INSTALL/VASP_GCC/AOCL/build
    1
    AOCL INSTALLED SUCCESSFULLY AT: /home/vincent/INSTALL/VASP_GCC/AOCL/build/4.2.0/aocc/lib
    source /home/vincent/INSTALL/VASP_GCC/AOCL/build/4.2.0/aocc/amd-libs.cfg
# $\color{Violet}{\textbf{Install OpenMPI 4}}$ 
    cd ../../
    mkdir OpenMPI
    cp openmpi-4.1.6.tar.gz OpenMPI
    cd OpenMPI
    mkdir OpenMPI_AOCC
    tar xvzf openmpi-4.1.6.tar.gz -C OpenMPI_AOCC
    cd OpenMPI_AOCC
    cd openmpi-5.0.5
    mkdir build
    cd build
    pwd
#### gives
    /home/vincent/INSTALL/VASP_GCC/OpenMPI/OpenMPI_AOCC/openmpi-4.1.6/build
#### copy 
    cd ..
    ./configure CC=clang CXX=clang++ FC=flang F77=flang OMPI_CC=clang OMPI_CXX=clang++ OMPI_FC=flang OMPI_F77=flang --prefix=/home/vincent/INSTALL/VASP_GCC/OpenMPI/OpenMPI_AOCC/openmpi-4.1.6/build
### $\color{Red}{\textbf{Error}}$
 
\* It appears that your C++ compiler is unable to link against object 

\* files created by your C compiler.  This generally indicates either

\* a conflict between the options specified in CFLAGS and CXXFLAGS

\* or a problem with the local compiler installation.  More

\* information (including exactly what command was given to the

\* compilers and what error resulted when the commands were executed) is

\* available in the config.log file in this directory.

\**********************************************************************

configure: error: C and C++ compilers are not link compatible.  Can not continue.
### $\color{Green}{\textbf{Fix}}$
#### Open a terminal
    sudo apt install g++-12
#### This solved the problem

# $\color{Violet}{\textbf{Install OpenMPI 5}}$ 
    mkdir OpenMPI
    cp openmpi-5.0.5.tar.gz OpenMPI
    cd OpenMPI
    mkdir OpenMPI_AOCC
    tar xvzf openmpi-5.0.5.tar.gz -C OpenMPI_AOCC
    cd OpenMPI_AOCC
    cd openmpi-5.0.5
    mkdir build
    cd build
    pwd
#### gives
     /home/vincent/INSTALL/VASP_GCC/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build
#### copy 
    cd ..
    ./configure CC=clang CXX=clang++ FC=flang F77=flang OMPI_CC=clang OMPI_CXX=clang++ OMPI_FC=flang OMPI_F77=flang --prefix=/home/vincent/INSTALL/VASP_GCC/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build
    make install
#### Local check
    cd build
    pwd
    copy path
#### Local activate
    export PATH=/home/vincent/INSTALL/VASP_GCC/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build/bin:$PATH
    export LD_LIBRARY_PATH=/home/vincent/INSTALL/VASP_GCC/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build/lib:$LD_LIBRARY_PATH

    
# $\color{Violet}{\textbf{Install HDF5}}$  
    cd ../../../../
    mkdir HDF5
    cd HDF5
    mkdir HDF5_AOCC
    cd HDF5_AOCC
    cd ../../
    cp hdf5-1.14.4-3.tar.gz HDF5/HDF5_AOCC/
    cd HDF5/HDF5_AOCC
    tar xvzf hdf5-1.14.4-3.tar.gz
    cd hdf5-1.14.4-3
    mkdir build
    cd build
        
### copy path
    pwd
### gives
     /home/vincent/INSTALL/VASP_GCC/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build
    cd ..
#### Then execute the following to build HDF5
    ./configure --enable-fortran CC=clang CXX=clang++ FC=flang F77=flang OMPI_CC=clang OMPI_CXX=clang++ OMPI_FC=flang OMPI_F77=flang --prefix=/home/vincent/INSTALL/VASP_GCC/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build
    make 
    make install
#### OR
#### Execute the following to build parallel HDF5
    ./configure --prefix=/home/vincent/INSTALL/VASP_GCC/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build CC=mpicc --enable-fortran --enable-parallel
    make 
    make install
#### local activate
    export PATH=/home/vincent/INSTALL/VASP_GCC/HDF5/HDF5_AOCC/hdf5-1.14.4-3/build/bin:$PATH
    export LD_LIBRARY_PATH=/home/vincent/INSTALL/VASP_GCC/HDF5/HDF5_AOCC/hdf5-1.14.4-3/build/lib:$LD_LIBRARY_PATH

# $\color{Violet}{\textbf{Install Wannier90}}$  
#### $\color{Red}{\textbf{We cannot use wannier with flang}}$
#### We have tried Editing make file with flang, commenting fcopts, ... but all tests will fail
#### $\color{Green}{\textbf{Make wannier with the added makefile ``make.inc''}}$
    cd ../../../
    mkdir Wannier
    cp wannier90-3.1.0.tar.gz Wannier
    cd Wannier
    tar xvzf wannier90-3.1.0.tar.gz
    cd wannier90-3.1.0
    make
    make install
    make tests 
#### local activate
    export PATH=/home/vincent/INSTALL/wannier90-3.1.0:$PATH
    export LD_LIBRARY_PATH=/home/vincent/INSTALL/wannier90-3.1.0:$LD_LIBRARY_PATH
# $\color{Violet}{\textbf{Install VASP}}$  
    cd ../../
    mkdir VASP
    cp vasp.6.4.0_0.tgz VASP
    cd VASP
    tar xvzf vasp.6.4.0_0.tgz
    cd vasp.6.4.0
    cd arch
    cp makefile.include.aocc_ompi_aocl_omp ../makefile.include

#### We have included the "makefile.include" in this repository so instead of the previous step just go to make
    cd ..
    make DEPS=1 -j
#### If you need to remake VASP
    make veryclean
    make DEPS=1 -j
#### SET ENV
    source /home/vincent/INSTALL/VASP_GCC/AOCC/setenv_AOCC.sh
    source /home/vincent/INSTALL/VASP_GCC/AOCL/build/4.2.0/aocc/amd-libs.cfg
    export PATH=/home/vincent/INSTALL/VASP_GCC/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build/bin:$PATH
    export LD_LIBRARY_PATH=/home/vincent/INSTALL/VASP_GCC/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build/lib:$LD_LIBRARY_PATH
    export PATH=/home/vincent/INSTALL/VASP_GCC/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build/bin:$PATH
    export LD_LIBRARY_PATH=/home/vincent/INSTALL/VASP_GCC/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build/lib:$LD_LIBRARY_PATH
    mpirun -np 1 /home/vincent/INSTALL/VASP_GCC/vasp6/vasp.6.4.0/bin/vasp_ncl > output &
#### Path of AOCL
    /home/vincent/INSTALL/VASP_GCC/AOCL/build/4.2.0/aocc
#### Following are some additional useful commands
#### to check no: of cores
    cat /proc/cpuinfo | grep "cpu cores" | uniq
##### In our case
    4
#### to check no: of logical cores
    cat /proc/cpuinfo | grep "processor" | wc -l
##### In our case
    8
#### This means that we have a total core 8, but in terms of 4 $\times$ 2
#### That is maximum value of $ncores = 4 in mpirun
    mpirun -np $ncores
#### Final running command for pure mpi parallelization (OMP_NUM_THREADS=1)
    mpirun -np 4 --map-by node:PE=1 --bind-to core -x OMP_NUM_THREADS=1 -x OMP_STACKSIZE=512m -x OMP_PLACES=cores -x OMP_PROC_BIND=close /home/vincent/INSTALL/VASP_gcc_omp/vasp.6.4.0/bin/vasp_std > output &
