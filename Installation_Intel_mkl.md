### VASP installation trial with GNU

#### Prerequisites

    cmake --version
    locate libopenmpi-dev
    locate openmpi-bin
    locate libfftw3-dev

#### Download tar file at the location VASP_gcc_ompi
# $\color{Violet}{\textbf{Install VASP}}$  
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
