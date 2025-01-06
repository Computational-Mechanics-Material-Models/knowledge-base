# Guide for installing Project Chrono on Windows Subsystem for Linux (WSL)
Last updated: 01/06/2025

## Dependencies

* install cmake: `sudo apt-get install cmake`

* install build essentials: `sudo apt-get install build-essential`

* install Eigen: `sudo apt-get install libeigen3-dev`

* install Irrlicht on WSL: `sudo apt-get install libirrlicht-dev`
    * install additional dependencies of Irrlicht:
		* GLUT: `sudo apt-get install libglut-dev`
		* X11 XFree86 video mode extension library: `sudo apt-get install libxxf86vm-dev`

* chrono module Pardiso-MKL requires Intel MKL library: `sudo apt-get install libmkl-dev`

* git comes pre-installed on WSL. If not already done:
    * set up your profile: `git config --global user.name <username>` and `git config --global user.email <email_address>`
	* make SSH keys for easier login into GitHub
        * go to SSH folder `cd ~/.ssh` and create key: `ssh-keygen -o -t rsa -C “ssh@github.com”`
        * display `cat id.rsa_pub` and copy public key into GitHub new SSH key (Settings/SSH and GPG Keys)

## Chrono code

* clone ProjectChrono: `git clone git@github.com:projectchrono/chrono.git` and go into the chrono directory: `cd chrono`

* install chrono Release version 8.0:
    * create a build directory `mkdir build_release_8.0` and go into it `cd build_release_8.0`
    * checkout the release 8.0 code: `git checkout --track origin/release/8.0`
    * configure the build with cmake: `cmake .. -D Eigen3_DIR:PATH=/usr/share/eigen3/cmake -D ENABLE_MODULE_IRRLICHT=ON -D IRRLICHT_ROOT=/usr/include/irrlicht -D IRRLICHT_LIBRARY=/usr/lib/x86_64-linux-gnu/libIrrlicht.so -D ENABLE_MODULE_PARDISO_MKL=ON -D MKL_ROOT:PATH=/usr/include/mkl -D PTHREAD_LIBRARY=/usr/lib/x86_64-linux-gnu/libpthread.a -D CMAKE_INSTALL_PREFIX=bin`
    * build `make` and install `make install` the code

* install chrono Release version 9.0 (nearly the same process except for some slight CMake variable name changes): 
    * create a build directory `mkdir build_release_9.0` and go into it `cd build_release_9.0`
    * checkout the release 9.0 code: `git checkout --track origin/release/9.0`
    * configure the build with cmake: `cmake .. -D Eigen3_DIR:PATH=/usr/share/eigen3/cmake -D ENABLE_MODULE_IRRLICHT=ON -D IRRLICHT_INSTALL_DIR=/usr/include/irrlicht -D IRRLICHT_LIBRARY=/usr/lib/x86_64-linux-gnu/libIrrlicht.so -D ENABLE_MODULE_PARDISO_MKL=ON -D MKL_ROOT:PATH=/usr/include/mkl -D PTHREAD_LIBRARY=/usr/lib/x86_64-linux-gnu/libpthread.a -D CMAKE_INSTALL_PREFIX=bin`
    * build `make` and install `make install` the code

If there are any issues with configuration using the CMake command line, `ccmake` and `cmake-gui` may be used alternatively
following the guidelines from the [Chrono tutorial](https://api.projectchrono.org/tutorial_install_chrono.html). `ccmake`
and `cmake-gui` require additional packages to be installed (not documented in the Dependencies section above, TODO).
    
