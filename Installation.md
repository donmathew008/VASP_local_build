### VASP installation trial with AOCC and AOCL locally
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
        cp openmpi-5.0.5.tar.gz OpenMPI
        cd OpenMPI
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
        







