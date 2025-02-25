# Guide for installing Project Chrono on Windows Subsystem for Linux (WSL)
Last updated: 02/21/2025 (checked on `chrono` commit ed211c7c91db673bd3334ffe877cbdedc2efae31)

## Pre-requisites

* install cmake: `sudo apt-get install cmake`

* install build essentials: `sudo apt-get install build-essential`

* install Ninja build system: `sudo apt install ninja-build`

* install unzip: `sudo apt install unzip`

* git comes pre-installed on WSL. If not already done:
    * set up your profile: `git config --global user.name <username>` and `git config --global user.email <email_address>`
	* make SSH keys for easier login into GitHub
        * go to SSH folder `cd ~/.ssh` and create key: `ssh-keygen -o -t rsa -C “ssh@github.com”`
        * display `cat id.rsa_pub` and copy public key into GitHub new SSH key (Settings/SSH and GPG Keys)

## CCS CBL Chrono

* go into your home directory: `cd ~`

* clone chrono-cbl: `git clone git@github.com:Cusatis-Computational-Services/chrono-cbl.git` and go into the chrono-cbl directory: `cd chrono-cbl`

* get submodules: `git submodule init`, followed by `git submodule update`

# Chrono basic dependencies and third parties

Chrono requires dependencies that are easier to install using 
the methods and build scripts provided by Chrono that places all files in a `Packages` folder

* go into your home directory: `cd ~` and create Packages directory: `mkdir Packages`

* install Irrlicht:
    * download [Irrlicht 1.8.5](http://downloads.sourceforge.net/irrlicht/irrlicht-1.8.5.zip)
        * the zip file will likely be in your Windows 'Downloads' folder "/mnt/c/Users/<windows_username>/Downloads", where `<windows_username>` should be replaced by your Windows username.
    * unzip into the Packages folder: `unzip /mnt/c/Users/<windows_username>/Downloads/irrlicht-1.8.5.zip -d Packages`

* install Pardiso-MKL: Intel oneMKL is not always available from APT on WSL. Instead go to the [oneMKL webpage](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl-download.html?operatingsystem=linux&linux-install=online) and copy and paste the "Command Line Download" instructions.

* go into the build-scripts directory `cd chrono-cbl/contrib/build-scripts/linux` and enable script execution `chmod +x *`

* install Eigen: `./buildEigen.sh`


# Chrono advanced dependencies and third parties

If you wish to build more modules and third party dependencies. This is not necessary / incomplete as of right now:

* install CUDA (for FSI, GPU, SENSOR, MULTICORE, modules): [CUDA webpage](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local) and copy and paste the "Installation instructions".

* install MUMPS (for MUMPS module): `./buildMUMPS.sh`

* install Spectra (for MODAL module): `./buildSpectra.sh`

* install VSG (for VSG module, failed to build on my platform): `./buildVSG.sh`

* install Blaze (for MULTICORE module): `./buildBlaze.sh`

# Chrono installation

* go into chrono-cbl directory: `cd ~/chrono-cbl`
* install chrono-cbl (enable testing for unit and benchamark test during development): 
    * the default branch should be `cbl`, if not, checkout the `cbl` branch: `git checkout --track origin/cbl`
    * configure the build with json preset: `cmake --preset wsl_cbl_light`
    * go into build directory: `cd build` and build chrono-cbl: `ninja`

If there are any issues with configuration using the CMake command line, `ccmake` and `cmake-gui` may be used alternatively
following the guidelines from the [Chrono tutorial](https://api.projectchrono.org/tutorial_install_chrono.html). `ccmake`
and `cmake-gui` require additional packages to be installed (not documented in the Dependencies section above, TODO).


