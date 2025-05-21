# Guide for installing Chrono on Windows Subsystem for Linux (WSL)
Last updated: 05/20/2025 

## Installing chrono

Follow these steps if you are installing chrono for the first time.

### System pre-requisites

* install cmake: `sudo apt-get install cmake`

* install build essentials: `sudo apt-get install build-essential`

* if using the Ninja build system, install it: `sudo apt install ninja-build`

* install unzip: `sudo apt install unzip`

* git comes pre-installed on WSL. If not already done:
    * set up your profile: `git config --global user.name <username>` and `git config --global user.email <email_address>`
	* make SSH keys for easier login into GitHub
        * go to SSH folder `cd ~/.ssh` and create key: `ssh-keygen -o -t rsa -C “ssh@github.com”`
        * display `cat id.rsa_pub` and copy public key into GitHub new SSH key (Settings/SSH and GPG Keys)

### Chrono repository

* go into your home directory: `cd ~`

* clone chrono-mechanics: `git clone git@github.com:Computational-Mechanics-Material-Models/chrono-mechanics.git`
and go into the chrono-mechanics directory: `cd chrono-mechanics`

* install submodules: `git submodule update --init --recursive`

* download the preset file `chrono/CMakeUserPresets.json` in this `knowldge-base` repository and copy it into the `chrono-mechanics` directory

### Chrono basic dependencies and third parties

Chrono requires dependencies that are easier to install using 
the methods and build scripts provided by Chrono that places all files in a `Packages` folder

* go into your home directory: `cd ~` and create Packages directory: `mkdir Packages`

* install Irrlicht:
    * download [Irrlicht 1.8.5](http://downloads.sourceforge.net/irrlicht/irrlicht-1.8.5.zip)
        * the zip file will likely be in your Windows 'Downloads' folder "/mnt/c/Users/<windows_username>/Downloads", where `<windows_username>` should be replaced by your Windows username.
    * unzip into the Packages folder: `unzip /mnt/c/Users/<windows_username>/Downloads/irrlicht-1.8.5.zip -d Packages`

* install Pardiso-MKL: Intel oneMKL is not always available from APT on WSL. Instead go to the [oneMKL webpage](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl-download.html?operatingsystem=linux&linux-install=online) and copy and paste the "Command Line Download" instructions.

* go into the build-scripts directory `cd chrono-mechanics/contrib/build-scripts/linux` and enable script execution `chmod +x *`

* install Eigen: `./buildEigen.sh`

### Chrono advanced dependencies and third parties (optional)

If you wish to build more modules and third party dependencies. This is not necessary / incomplete as of right now:

* install CUDA (for FSI, GPU, SENSOR, MULTICORE, modules): [CUDA webpage](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local) and copy and paste the "Installation instructions".

* install MUMPS (for MUMPS module): `./buildMUMPS.sh`

* install Spectra (for MODAL module): `./buildSpectra.sh`

* install VSG (for VSG module, failed to build on my platform): `./buildVSG.sh`

* install Blaze (for MULTICORE module): `./buildBlaze.sh`


### CBL

* go into chrono-mechanics directory: `cd ~/chrono-mechanics`
* the default branch should be `main`, checkout the `cbl_dev` branch: `git checkout --track origin/cbl-dev`
* configure the build with the json preset:
    * for Ninja: `cmake --preset wsl_cbl_ninja`
    * for Unix Makefiles: `cmake --preset wsl_cbl_unix`
* go into build directory: `cd build` and build the code:
    * for Ninja: `ninja`
    * for Unix Makefiles: `make`

### LDPM (TODO currently not up to date)

* go into chrono-mechanics directory: `cd ~/chrono-mechanics`
* the default branch should be `main`, checkout the `ldpm_dev` branch: `git checkout --track origin/ldpm_dev`
* configure the build with the json preset: `cmake --preset wsl_ldpm_light`
* go into build directory: `cd build` and build the code: `ninja`


[TODO: make presets for ldpm]:#
If there are any issues with configuration using the CMake command line, `ccmake` and `cmake-gui` may be used alternatively
following the guidelines from the [Chrono tutorial](https://api.projectchrono.org/tutorial_install_chrono.html). `ccmake`
and `cmake-gui` require additional packages to be installed (not documented in the Dependencies section above, TODO).


## Updating chrono

Updating your CBL or LDPM code requires understanding the structure of the `chrono-mechanics repository shown below:

```
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ main  
    \                    
     \                      _ _ cbl_dev
      \                    /
       \_ _ _ base_dev _ _/
                          \
                           \_ _ ldpm_dev
```

The branches work as follows:
* the `main` branch tracks ProjectChrono/chrono's `main` branch to get the updates
    * :warning: DO NOT PUSH TO THIS BRANCH :warning:
* the `base_dev` branch includes our internal developments that are:
    * common to LDPM and CBL
    * not yet merged into ProjectChrono/chrono
* the `cbl_dev` branch includes the code for CBL 
* the `ldpm_dev` branch includes the code for LDPM

The tree is organized as follows:
* the `cbl_dev` and `ldpm_dev` branches are based on the `base_dev` branch 
* the `base_dev` branche is based on the `main` branch 

Pulling the latest updates for LDPM or CBL requires pulling
the latest updates from all parent branches along the tree:
* go into chrono-mechanics directory: `cd ~/chrono-mechanics`
* checkout the `main` branch: `git checkout main` and pull latest updates: `git pull`
* checkout the `base_dev` branch: `git checkout base_dev` and pull latest updates: `git pull`
* for LPDM: checkout the `ldpm_dev` branch: `git checkout ldpm_dev` and pull latest updates: `git pull`
* for CBL: checkout the `cbl_dev` branch: `git checkout cbl_dev` and pull latest updates: `git pull`

