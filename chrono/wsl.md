# Guide for installing Project Chrono on Windows Subsystem for Linux (WSL)
Last updated: 02/21/2025 (checked on `chrono` commit ed211c7c91db673bd3334ffe877cbdedc2efae31)

## Dependencies

* install cmake: `sudo apt-get install cmake`

* install build essentials: `sudo apt-get install build-essential`

* install Eigen: `sudo apt-get install libeigen3-dev`

* install Irrlicht on WSL: `sudo apt-get install libirrlicht-dev`
    * install additional dependencies of Irrlicht:
		* GLUT: `sudo apt-get install libglut-dev`
		* X11 XFree86 video mode extension library: `sudo apt-get install libxxf86vm-dev`

* chrono module Pardiso-MKL requires Intel oneMKL which is not available from APT. Instead go to the [oneMKL webpage](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl-download.html?operatingsystem=linux&linux-install=online) and copy and paste the "Command Line Download" instructions.

* git comes pre-installed on WSL. If not already done:
    * set up your profile: `git config --global user.name <username>` and `git config --global user.email <email_address>`
	* make SSH keys for easier login into GitHub
        * go to SSH folder `cd ~/.ssh` and create key: `ssh-keygen -o -t rsa -C “ssh@github.com”`
        * display `cat id.rsa_pub` and copy public key into GitHub new SSH key (Settings/SSH and GPG Keys)

## CCS CBL Chrono code

* clone chrono-cbl: `git clone git@github.com:Cusatis-Computational-Services/chrono-cbl.git` and go into the chrono-cbl directory: `cd chrono-cbl`

* get submodules: `git submodule init`, followed by `git submodule update`

* install chrono-cbl (enable testing for unit and benchamark test during development): 
    * create a build directory `mkdir build` and go into it `cd build`
    * the default branch should be `cbl`, if not, checkout the `cbl` branch: `git checkout --track origin/cbl`
    * configure the build with cmake: `cmake .. -D Eigen3_DIR:PATH=/usr/share/eigen3/cmake -D CH_ENABLE_MODULE_WOOD=ON -D CH_ENABLE_MODULE_IRRLICHT=ON -D CH_ENABLE_MODULE_PARDISO_MKL=ON -D MKL_DIR:PATH=/opt/intel/oneapi/mkl/latest/lib/cmake/mkl -D CMAKE_INSTALL_PREFIX=bin -D BUILD_TESTING=ON -D CMAKE_BUILD_TYPE=Release`
    * build `make -j 12` and install `make install` the code (the `-j 12` builds faster using multiple (here 12) jobs)


If there are any issues with configuration using the CMake command line, `ccmake` and `cmake-gui` may be used alternatively
following the guidelines from the [Chrono tutorial](https://api.projectchrono.org/tutorial_install_chrono.html). `ccmake`
and `cmake-gui` require additional packages to be installed (not documented in the Dependencies section above, TODO).


