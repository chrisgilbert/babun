# Babun - a Windows shell you will love!

Would you like to use a linux-like console on a Windows host without a lot of fuzz? Do you like oh-my-zsh?
Stop hating Windows and try out babun - you will simply love it!

### Features in 10 seconds

Babun features the following:
* Pre-configured Cygwin with a lot of addons
* Silent command-line installer, no admin rights required
* pact - advanced package manager (like apt-get or yum)
* xTerm-256 compatible console
* HTTP(s) proxying support
* Plugin-oriented architecture
* Pre-configured git and shell
* Integrated .oh-my-zsh
* Auto update

### Features in 3 minutes

##### Cygwin

The core of Babun consists of a pre-configured Cygwin. Cygwin is a great tool, but there's a lot of quirks and tricks that makes you loose a lot of time to make it actually "usable". Not only does babun solve most of these problems, but also contains a lot of vital packages, so that you can be productive from the very first minute. 

##### Package manager

Babun provides a package manager called 'pact'. It is similar to 'apt-get' or 'yum'. Pact enables installing/searching/upgrading and deinstalling cygwin packages with no hassle at all. Just invoke 'pact --help' to check how to use it.

##### Shell

Babun's shell is tweaked in order to provide the best possible user-experience. There are two shell types that are pre-configured and avaiable right away - bash and zsh (zsh is the default one). Babun's shell features:
* syntax highlighting
* unix tools
* software development tools
* git-aware prompt 
* custom scripts and aliases
* and much more!

##### Console

Mintty is the console used in babun. It features an xterm-256 mode, nice fonts and simply looks great!

##### Proxying

Babun supports HTTP proxying out of the box. Just add the address and the credentials of your HTTP proxy server to the .babun.rc file located in your home folder and execute 'source .babunrc' to enable HTTP proxying. SOCKS proxies are not supported for now.

##### Developer tools

Babun provides many packages, convenience tools and scripts that make your life much easier. The long list of features includes:
* programming languages (python, perl, groovy, etc.)
* git (with a wide variety of aliases and tweaks)
* unix tools (grep, wget, curl, etc.)
* oh-my-zsh
* custom scripts (pbcopy, pbpaste, babun, etc.)

##### Plugin architecture

Babun has a very small microkernel (cygwin, a couple of bash scripts and a bit of a convention) and a plugin architecture on the top of it. It means that almost everything is a plugin in the babun's world! Not only does it structure babun in a clean way, but also enables others to contribute small chunks of code - plugins. 
Currently, babun comprises of six plugins:
* cacert
* core
* git
* oh-my-zsh
* pact
* shell

##### Auto-update

Self-update is at the very heart of babun! Many Cygwin tools are simple bash scripts - once you install them there is no chance of getting the newer version in a smooth way. You either delete the older version or overwrite it with the newest one loosing all the changes you have made in between.

Babun contains an auto-update feature which enables updating both the microkernel and the plugins. Files located in your home folder will not be delted nor overwritten - which preserves your config and customizations.

##### Installer

Babun features an silent command-line installation script that may be executed without admin rights on Windows hosts.

### Installation

Just download the dist file from TODO, unzip it and run the install.bat script.
It's as simple as it gets!

### Project Sturcture

The project consists of four main modules.

##### babun-packages

The main goal of the babun-packages module is to download the cygwin packages listed in the conf/cygwin.x86.packages file.
The mentioned packages will be downloaded together with the whole dependency tree. Repositories which the packages are downloaded from are listed in the conf/cygwin.repositories file. At the beginning the first repository is taken, if a package is not available in this repo the second repo is used, etc. The process continues untill all packages have been downloaded. 

All downloaded packages are stored in the target/babun-packages folder.

##### babun-cygwin

The main goal of the babun-cygwin module is to download and invoke the native cygwin.exe installer. The packages downloaded by the babun-packages module are used as the input - all of them will be installed in the offline cygwin installation. 

It is not trivial to install and zip a local instance of Cygwin - there are problems with the symlinks as the symlink-file-flags are lost during the compresion proces. Babun can work it around though. At first, just after the installaion, the symlinks_find.sh script is invoked in order to store the list of all cygwin's symlinks. This file is delivered in the babun's core. Then, after babun is installed from the zip file on the user's host the symlinks_repair.sh script is invoked - it will correct all the broken symlinks listed in the abovementioned file.

Preinstalled cygwin is located in the target/babun-cygwin folder.

##### babun-core

The main goal of the babun-core module is to install babun's core along with all the plugins. install.sh script is invoked during the creation of the distribution package in order to preinstall the plugins. Whenver babun is installed on the user's host the install_home.sh script is invoke in order to install the babun-related files to the cygwin-user's home folder.

Preinstalled cygwin with installed babun is located in the target/babun-cygwin folder.

##### babun-dist

The main goal of the babun-dist module is to zip the ready-made instance of babun and copy some installation scripts on the top.
After the build has finished babun zip distribution is ready!

Distribution package is located in the target/babun-dist folder.

### Development

##### Building from source

The project is regularly build on Jenkins, on a slave node running Windows. The Windows OS is required to fully build the distribution package as one of the goals invokes the native Cygwin installer. The artifacts created by each module are cached in the target folder after a successful build. This mechanism is not intelligent to calculate the diffs so if you would like to fully rebuild a module make sure to invoke the "clean" goal before the "package" goal. For now it's not possible to invoke a build of a selective modules only. 

In order to build the dist package invoke:
```
groovy build.groovy package 
```

In order to clean the project target folder invoke:
```
groovy build.groovy clean 
```

In order to publish the release version to bintray invoke:
```
groovy build.groovy release
```
The release goal expects the following environment variables: bintray_user and bintray_secret


### Contribute

We are looking for new contributors, so if you fancy bash progamming and if you would like to contribute a patch or a code up a new plugin give us a shout!


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/reficio/babun/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

