# CDL tools grip repository

This is a grip repository that provides all the tools required for the
ATCF hardware CDL repositories for use in simulation and for building
FPGAs.

As a grip repository it is a meta-repository; it does not *include*
the big repos that it uses, but it references them. To use this repo
requires the 'grip' tool; this tool is *also* included in this grip
repository, but an external 'grip' checkout is required to bootstrap.

## Purpose

The repository is provided so that the use of the ATCF hardware (and
similar repositories) can be enabled with a single download to known
working versions of tools.

This means that a user does not have to follow a script to get
binutils form one place and install it somewhere, get gcc from
somewhere else, etc; then discover the tools are not compatible, and
rebuild one or the other, or both; and so on.

## Tools included

The tools that this repository builds are:

* CDL 2.0 - the cdl to C and Verilog hardware description language and
build system with Python-3 test harness support

* Verilator - the open source Verilog/SystemVerilog to C++ compiler,
currently using v4.028 as that is known to work

* Binutils - for assembling and linking for RISC-V

* GNU toolchain - for cross-compiling to RISC-V

## Grip configurations

There is one main configuration: 'all'. This downloads and installs all of the tools.
The tools are built in to TOOLS_DIR, which defaults to <cd>/tools.

## Prerequisites

Builds require:

* autoconf (which pulls in automake autotools-dev on Ubuntu)

* bison

* flex

* python3

* pip3

* pip3 install toml

Some of the ATCF hardware builds require libpng and libjpeg - there
should be mechanisms for ensuring these are present, but that is work-in-progress.

# Using this repository

To use this repository, clone it and then grip configure it.

```
cd <somewhere>
git clone https://github.com/atthecodeface/cdl_tools_grip.git
grip configure all
```

Once configured (a process in grip which will clone the required
subrepositories), an install can be performed:

```
grip install
```

The installation will:

* download the binutils and gcc tarballs required

* configure these, and download (for gcc) more things (the nature of
gcc building)

* configure cdl and verilator

* compile binutils, gcc, verilator and cdl

* install them in to <cdl_tools_grip>/tools

At this point all the tools are ready for use.

## Bootstrapping - "grip not found"

To be able to do that initial 'grip configure' may require a download
of grip itself *FIRST*:

```
cd <somewhere>
git clone https://github.com/atthecodeface/grip.git
cd <somewhere>/cdl_tools_grip
<somewhere>/grip/grip configure all
```

# An example of use

With the cdl_tools_grip repository configured and installed, the
atcf_hardware_basic_grip repository can be used.

## Check out and configure the atcf_hardware_basic_grip.git

This is a one-time checkout:

```
export ATCF_CDL_TOOLS_PATH <somewhere>/cdl_tools_grip/tools
cd <somewhere>
git clone https://github.com/atthecodeface/atcf_hardware_basic_grip.git
$ATCF_CDL_TOOLS_PATH/cdl_tools_grip/grip configure
```

## Use the repository

The best way to work within a grip repository is using 'grip shell' -
this will start a bash shell with the environment configured *as
required* by that grip repository - paths, environment variables, etc.

```
cd <somewhere>/atcf_hardware_basic_grip
<somewhere>/cdl_tools_grip/grip shell
```

Inside this shell *grip* will be on the path, as will verilator, cdl,
and the GNU cross compilers, and so on.

Now:

```
grip make test
```

will compile, link etc, and run the tests.

For more details, see the Readme.md in atcf_hardware_basic_grip

