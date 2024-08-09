### VASP installation trial with AOCC and AOCL locally
    cmake --version
    locate libopenmpi-dev
    locate openmpi-bin
    locate libfftw3-dev

##### Go to AOCC extracted location and open a terminal
cd INSTALL/AOCC_latest/aocc-compiler-4.2.0
    ./install.sh
##### Copy and save the setup code(IMPORTANT), here for our case
    source /home/vincent/INSTALL/AOCC_latest/setenv_AOCC.sh

##### To check installation run
    source /home/vincent/INSTALL/AOCC_latest/setenv_AOCC.sh
    ./AOCC-prerequisites-check.sh 

## Fix the errors(For our case)

    sudo apt install --reinstall libquadmath0
#### Not solved

    sudo cp /usr/lib/x86_64-linux-gnu/libquadmath.so.0 /usr/lib32
    sudo ldconfig
#### Still not solved, just moving forward
