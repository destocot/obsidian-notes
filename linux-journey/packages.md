---
title: "Packages"
sidebar:
  order: 12
---

## Software Distribution

**Upstream providers** - people (or sometimes a single person) that write software. They compile their code and write up how it gets installed.

**Package maintainers** - Handle getting this piece of software in the hands of users. These package maintainers review, manage, and distribute this software in the form of packages.

## Package Repositories

On a Debian system, this sources file is the `/etc/apt/soruces.list` file. Your machine will know to look there and check for any source repositories you added.

## tar and gzip

### Compressing files with gzip

**gzip** is a program used to compress files in Linux, they end in a `.gz` extension.

```bash
gzip mycoolfile
# compress a file down

gunzip mycoolfile.gz
# decompress the file
```

### Creating archives with tar

Unfortunately **gzip** can't add multiple files into one archive for us. Luckily we have the tar program which does.

```bash
tar cvf mytarfile.tar mycoolfile1 mycoolfile2
# c - create
# v - tell the program to be vervose and let u see what it's doing
# f - the filename of the tar file has to come after this option
```

### Unpacking archives with tar

```bash
tar xvf mytarfile.tar
# x - extract
# v - tell the program to be verbose and let us see what it's doing
# f - the file you want to extract
```

### Compressing/uncompressing archives with tar and gzip

```bash
tar czf myfile.tar.gz
# create a compressed tar file

tar xzf file.tar
# uncompress and unpack
```

> `z` option tells **tar** to use it with the **gzip** or **gunzip** utility

_remember_: e**X**tract all **Z** **F**iles!

## Package Dependencies

Packages very rarely work by themselves, they are most often accompanied by dependencies to help them run.

In Linux, these dependences are often other packages or shared libraries. Shared libraries are libraries of code that other programs want to use and don't want to have to rewrite for themselves.

## rpm and dpkg

To install packages such as **.deb** and **.rpm** we would use their respective package management commants `rpm` and `dpkg`.

This commands will install the package files, _HOWEVER_ they will not install the package dependencies.

### Install a package

```bash
dpkg -i some_deb_package.deb # Debian
rpm -i some_rpm_package.rpm # RPM
# the -i (install) flag is used to install the package
```

### Remove a package

```bash
dpkg -r some_deb_package.deb # Debian
# the -r (remove) flag removes a debian package

rpm -e some_rpm_package.deb # RPM
# the -e (erase) flag removes a rpm package
```

### List installed packages

```bash
dpkg -l # Debiran
# the -l (list) flag lists debian packages installed

rpm -qa # RPM
# the -q (query) and -a (all) flags list rpm packages installed
```

## yum and apt

The `yum` and `apt` systems come with all the fixins to make package installation, removal and changes easier, including installing package dependencies.

- **yum** is exclusive to the Red Hat family
- **apt** is exclusive to the Debian family

### Install a package from a repository

```bash
apt install package_name # Debian

yum install package_name # RPM
```

### Remove a package

```bash
apt remove package_name # Debian

yum erase package_name # RPM
```

### Updating packages for a repository

```bash
apt update; apt upgrade # Debian

yum update # RPM
```

### Get information about an installed package

```bash
apt show package_name # Debian

yum info package_name # RPM
```

## Compile Source Code

Often times you will encouter an obsure package that only comes in the form of pure source code.

```bash
sudo apt install build-essential
# software to install the tools that will allow you to compile source code
```

```bash
tar -xzvf package.tar.gz
# extract the contents of the package file (most likely a .tar.gz file)
```

> Depending on the compile method that the developer used (look at **README** or **INSTALL** file), you'll have to use different commands. However, most commonly you'll see basic make compilation, so we'll discuss that.

```bash
./configure
# configure script checks for dependencies on your system
```

```bash
make
# the make command will look at the Makefile to build the software
# the Makefile contains the rules to build the software

sudo make install
# this command will actually install the package, will copy the correct files to the correct locations on your computer

sudo make uninstall
# uninstall the package
```

### checkinstall

Be wary when using `make install`, if you decide to remove this package it may not actually remove everything.

Instead use `checkinstall` this will make a `.deb` file that you can easily install and uninstall.

```bash
sudo checkinstall
# will `make install` and build a `.deb` package and install it
```
