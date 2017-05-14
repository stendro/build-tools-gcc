# GCC toolchain build script

This is a script to build GCC toolchains targeting arm and arm64 devices
(primarily Android devices).


## Using the script

To build a toolchain, you will need to the
following:

+ A Linux distribution (the script has been tested on Ubuntu 17.04 and Arch Linux)
+ A decent processor and RAM (i5 and 8GB of RAM or more is preferred)
+ Core developer packages
    + For Arch: the base-devel package should be enough
    + For Ubuntu: ```sudo apt-get install flex bison ncurses-dev texinfo gcc gperf patch libtool automake g++ libncurses5-dev gawk subversion expat libexpat1-dev python-all-dev binutils-static libgcc1:i386 bc libcloog-isl-dev libcap-dev autoconf libgmp-dev build-essential gcc-multilib g++-multilib pkg-config libmpc-dev libmpfr-dev```

Once you have set up your environment, run the following:

```bash
git clone https://github.com/nathanchance/build-tools-gcc
cd build-tools-gcc
bash build -h
```

The printout will show you how to run the script. A fuller explananation of the
parameters is below


## Parameters

NOTE: Multiple parameters can and will almost always be used. See examples below.

+ ```-a | --arm:``` This allows the user to build an arm toolchain, instead of an arm64 toolchain.
+ ```-b | --build:``` Build the toolchain. This parameter MUST be added for the toolchain to build if you add any parameters. Running just ```bash build``` will use this.
+ ```-c | --clean:``` Clean up from a previous build. Do this after the first download after every build.
+ ```-d | --download:``` Downloads the necessary components to compile. This MUST be added before building for the first time.
+ ```-h | --help:``` Run the help menu then exits. Use this whenever you forget how to run the script.
+ ```-l | --linaro:``` Build a Linaro toolchain from the latest release branch, instead of the standard GNU source.
+ ```-nt | --no-tmpfs:``` Do not mount the build directories on tmpfs (use this if you don't have a lot of RAM)
+ ```-u | --update:``` This will update the git repos that were downloaded. Recommended before any build.

Example commands:

```bash
# Build a Linaro toolchain for the first time
# Download components, checkout Linaro, then build
bash build -d -l -b

# Build a GNU toolchain after a few compilations
# Update sources, clean previous compilation, then build
bash build -u -c -b
```

If you want to switch between GNU and Linaro builds, ALWAYS run update so that
it checks out the proper branch.


## After compilation

Once it is done building, you will see a tar.xz file. Move that into the
directory of your choosing and run the following command:

```bash
tar -xvf --strip-components=1 <toolchain_name>.tar.xz
```

After that, point your cross compiler to the proper file and compile!

An easy shortcut for kernels is ```export CROSS_COMPILE=$(pwd)/bin/aarch64-linux-gnu-```
Subsititue ```arm-gnu-eabi-``` if compiling for arm.


## Pull requests/issues

If you have any issues with this script, feel free to open an issue!

Pull requests are more than welcome as well. For pull requests, I will only
accept a particular coding style:

+ All variables are uppercased and use curly braces: ```${VARIABLE}``` instead of ```$variable```
+ Four spaces for indents
+ All conditions must be one line to start: ```if [[ conditions ]]; then```
+ Double brackets and single equal sign for string comparisons with if statements: ```if [[ ${VARIABLE} = "yes" ]]; then```


## Credits/thanks

+ [USBHost](https://github.com/USBhost): For the initial script
+ [frap129](https://github.com/frap129): For some modifications to update the script/components
+ [MSF-Jarvis](https://github.com/MSF-Jarvis): For testing the arm option
