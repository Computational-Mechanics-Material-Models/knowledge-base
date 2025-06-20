# Guide for installing Chrono on Windows Subsystem for Linux (WSL)
Last updated: 06/20/2025

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

* download the preset file `chrono/CMakeUserPresets.json` in this `knowldge-base` repository and copy it at the root of the `chrono-mechanics` directory

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

### Chrono Code

* go into chrono-mechanics directory: `cd ~/chrono-mechanics`
* the default branch should be `base_dev`, if not, checkout the `base_dev` branch: `git checkout --track origin/base_dev`

#### CBL

* configure the build with the CBL json preset:
    * for Ninja: `cmake --preset wsl_cbl_ninja`
    * for Unix Makefiles: `cmake --preset wsl_cbl_unix`
* go into build directory: `cd build` and build the code:
    * for Ninja: `ninja`
    * for Unix Makefiles: `make`

#### LDPM

* configure the build with the LDPM json preset:
    * for Ninja: `cmake --preset wsl_ldpm_ninja`
    * for Unix Makefiles: `cmake --preset wsl_ldpm_unix`
* go into build directory: `cd build` and build the code:
    * for Ninja: `ninja`
    * for Unix Makefiles: `make`

#### ALL MODELS

* configure the build with the json preset for ALL MODELS:
    * for Ninja: `cmake --preset wsl_all_ninja`
    * for Unix Makefiles: `cmake --preset wsl_all_unix`
* go into build directory: `cd build` and build the code:
    * for Ninja: `ninja`
    * for Unix Makefiles: `make`


If there are any issues with configuration using the CMake command line, `ccmake` and `cmake-gui` may be used alternatively
following the guidelines from the [Chrono tutorial](https://api.projectchrono.org/tutorial_install_chrono.html). `ccmake`
and `cmake-gui` require additional packages to be installed (not documented in the Dependencies section above, TODO).


## Updating chrono-mechanics

Updating your code requires understanding the structure of the `chrono-mechanics repository shown below:

```
o---o---o---o---o---o---o---o---o  main
     \
      o---o---o---o  base_dev
```

The branches work as follows:
* the `main` branch tracks ProjectChrono/chrono's `main` branch to get the updates
* the `base_dev` branch includes all our developments (CBL, LDPM, ...) that are not present into ProjectChrono/chrono

To pull the latest updates:
* go into chrono-mechanics directory: `cd ~/chrono-mechanics`
* checkout the `main` branch: `git checkout main` and pull latest updates: `git pull`
    * this is required since this branch is ocasionally synced to ProjectChrono/chrono's `main` branch
* checkout the `base_dev` branch: `git checkout base_dev` and pull latest updates: `git pull --rebase`
    * pulling with rebase is required since the `base_dev` branch is frequently rebased on the `main` branch

## Developing chrono

:warning: The `base_dev` branch is now protected, you can no longer push to it and must use Pull Requests :warning:

To implement new features:
* pull the latest updates (see above)
* create a new branch: `git checkout -b <branch_name>`
* edit files, add and commit
* push your branch: `git push -u origin <branch_name>`
* submit a Pull Request on GitHub

If changes to `base_dev` occured during your development:
* pull the latest updates (see above)
* rebase your branch: `git checkout <branch_name>` followed by `git rebase base_dev`
* force push your changes: `git push --force-with-lease`
    * :warning: Rebasing modifies the commits SHA. Even though no changes were made to `<branch_name>` :warning:
    * :warning: Force pushing is required to updated the remote branch ! :warning:
    * :warning: if multiple people work on `<branch_name>`, double check that no new code was pushed since you began that proces :warning:


### Pulling updates from ProjectChrono/chrono (Admin only)

The original ProjectChrono/chrono developments must be periodically integrated into chrono-mechanics.

Set the ProjectChrono/chrono repository as a remote: `git remote add upstream git@github.com:projectchrono/chrono.git`
(this should only be done once)

 * go to your `main` branch on `chrono-mechanics`: `git checkout main`
 * pull the lastest ProjectChrono/chrono updates: `git pull`
 * push the changes to `chrono-mechanics`: `git push`

Once this is done, the `base_dev` branch must be rebased onto the newly pulled, latest `main` commit:
* `git checkout base_dev` followed by `git rebase main`

TODO

