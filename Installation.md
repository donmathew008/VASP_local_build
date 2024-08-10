### VASP installation trial with AOCC and AOCL locally
[Ref](https://implant.fs.cvut.cz/)

    cmake --version
    locate libopenmpi-dev
    locate openmpi-bin
    locate libfftw3-dev

##### Go to AOCC extracted location and open a terminal

    cd /INSTALL/VASP_AMD/aocc-compiler-4.2.0
    ./install.sh
##### Copy and save the setup code(IMPORTANT), here for our case
    source /home/vincent/INSTALL/VASP_AMD/setenv_AOCC.sh

##### To check installation run
    source /home/vincent/INSTALL/AOCC_latest/setenv_AOCC.sh
    ./AOCC-prerequisites-check.sh 

## Fix the errors(For our case)

    sudo apt install --reinstall libquadmath0
#### Not solved

    sudo cp /usr/lib/x86_64-linux-gnu/libquadmath.so.0 /usr/lib32
    sudo ldconfig
#### Still not solved, just moving forward



####################FINAL
        mkdir AOCC
        cp aocc-compiler-4.2.0.tar AOCC
        cd AOCC
        tar -xvf aocc-compiler-4.2.0.tar
        cd aocc-compiler-4.2.0/
        ./install.sh
        source /home/vincent/INSTALL/VASP_AMD/AOCC/setenv_AOCC.sh
        which clang
        clang -v
        ./AOCC-prerequisites-check.sh

#### AOCL
        cd ../../
        mkdir AOCL
        cp aocl-linux-aocc-4.2.0.tar.gz AOCL
        
        sudo cp -r aocl-linux-aocc-4.2.0 AOCL
        cd AOCL
        tar -xvf aocl-linux-aocc-4.2.0.tar.gz
        mkdir build
        cd aocl-linux-aocc-4.2.0
        pwd
### gives
        /home/vincent/INSTALL/VASP_AMD/AOCL/aocl-linux-aocc-4.2.0
### copy path

        ./install.sh -t /home/vincent/INSTALL/VASP_AMD/AOCL/build
        1
        AOCL INSTALLED SUCCESSFULLY AT: /home/vincent/INSTALL/VASP_AMD/AOCL/build/4.2.0/aocc/lib
        source /home/vincent/INSTALL/VASP_AMD/AOCL/build/4.2.0/aocc/amd-libs.cfg
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
        /home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-4.1.6/build
#### copy 
        cd ..
        ./configure CC=clang CXX=clang++ FC=flang F77=flang OMPI_CC=clang OMPI_CXX=clang++ OMPI_FC=flang OMPI_F77=flang --prefix=/home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-4.1.6/build
 ##### error
 
* It appears that your C++ compiler is unable to link against object
* files created by your C compiler.  This generally indicates either
* a conflict between the options specified in CFLAGS and CXXFLAGS
* or a problem with the local compiler installation.  More
* information (including exactly what command was given to the
* compilers and what error resulted when the commands were executed) is
* available in the config.log file in this directory.
**********************************************************************
configure: error: C and C++ compilers are not link compatible.  Can not continue.
### Fix?
        ./configure CC=clang CFLAGS=-m64 CXX=clang++ FC=flang FCFLAGS=-m64 F77=flang OMPI_CC=clang OMPI_CXX=clang++ OMPI_FC=flang OMPI_F77=flang --prefix=/home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build
#### Open aterminal
        sudo apt install g++-12
#### This solved the problem
        mkdir OpenMPI

#### OpenMPI 5
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
        /home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build
#### copy 
        cd ..
        ./configure CC=clang CXX=clang++ FC=flang F77=flang OMPI_CC=clang OMPI_CXX=clang++ OMPI_FC=flang OMPI_F77=flang --prefix=/home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build
        make install
#### Local check
        cd build
        pwd
        copy path
#### Local activate
        export PATH=/home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build/bin:$PATH
        export LD_LIBRARY_PATH=/home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build/lib:$LD_LIBRARY_PATH
### HDF5
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
### shows
        /home/vincent/INSTALL/VASP_AMD/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build  #new

        cd ..
        ./configure --enable-fortran CC=clang CXX=clang++ FC=flang F77=flang OMPI_CC=clang OMPI_CXX=clang++ OMPI_FC=flang OMPI_F77=flang --prefix=/home/vincent/INSTALL/VASP_AMD/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build

        make 
        make install
### local activate
        export PATH=/home/vincent/INSTALL/VASP_AMD/HDF5/HDF5_AOCC/hdf5-1.14.4-3/build/bin:$PATH
        export LD_LIBRARY_PATH=/home/vincent/INSTALL/VASP_AMD/HDF5/HDF5_AOCC/hdf5-1.14.4-3/build/lib:$LD_LIBRARY_PATH

#### Wannier
        cd ../../../
        mkdir Wannier
        cp wannier90-3.1.0.tar.gz Wannier
        cd Wannier
        tar xvzf wannier90-3.1.0.tar.gz
        cd wannier90-3.1.0
## edit make file with flang, comment fcopts,..
        make
        make install
        make tests # all failed we will use other wannier
### local activate
        export PATH=/home/vincent/INSTALL/wannier90-3.1.0:$PATH
        export LD_LIBRARY_PATH=/home/vincent/INSTALL/wannier90-3.1.0:$LD_LIBRARY_PATH
##
        cd ../../
        mkdir VASP
        cp vasp.6.4.0_0.tgz VASP
        cd VASP



## Source
        tar xvzf vasp.6.4.0_0.tgz
        cd vasp.6.4.0
        cd arch
        cp makefile.include.aocc_ompi_aocl_omp ../makefile.include
# copy path of aocl
        /home/vincent/INSTALL/VASP_AMD/AOCL/build/4.2.0/aocc


        cd ..


#### HDF5 new
        ./configure --prefix=/home/vincent/INSTALL/VASP_AMD/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build CC=mpicc --enable-fortran --enable-parallel

#### SET ENV
        source /home/vincent/INSTALL/VASP_AMD/AOCC/setenv_AOCC.sh
        source /home/vincent/INSTALL/VASP_AMD/AOCL/build/4.2.0/aocc/amd-libs.cfg
        export PATH=/home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build/bin:$PATH
        export LD_LIBRARY_PATH=/home/vincent/INSTALL/VASP_AMD/OpenMPI/OpenMPI_AOCC/openmpi-5.0.5/build/lib:$LD_LIBRARY_PATH
        export PATH=/home/vincent/INSTALL/VASP_AMD/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build/bin:$PATH
        export LD_LIBRARY_PATH=/home/vincent/INSTALL/VASP_AMD/HDF5/HDF5_AOCC/myhdfstuff/hdf5-1.14.4-3/build/lib:$LD_LIBRARY_PATH
        
