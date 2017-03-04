# PkgMgr
Package Manager for Linux From Scratch

This is a package manager for system built using Linux From Scratch. It is based on btrfstools and hence will required your '/' to be formatted with btrfs filesystem. It uses btrfs's snapshot feature to install and then listout the files created during the installation.

## Features:

1. Install packages
2. List installed packages
3. List files installed by a package
4. Remove a package. It provides two modes. Temporary remove (which is default) and Permanent remove. As the name suggest, temporary remove will allow you to re-install/restore the package while permanent remove won't.

## Prerequisite:

1. / formatted with btrfs
2. btrfstools are installed i.e. btrfs command is available.

How and what to configure:

You'll need to configure following things before using this utility by modifying the /etc/pkgmgr.conf file.

1. BTRFS_SNAPDIR: Where btrfs will create snapshots
2. BTRFS_ROOT_SUBVOLID: Your root's subvolume id.
3. BTRFS_DEVICE: Which device represents your '/' partition. e.g. /dev/sda2
4. PKG_BASE_DIR: Where all package related things will be kept. e.g. /opt/pkgmgr. You'll need to create this directory structure.

## Usage:

###### 1. Install
After doing `configure` and `make` instead of doing `make install`, run pkgmgr as `pkgmgr install <package name>` as root (or using sudo). If the command to install the package is not `make install` or has some variation to `make install` e.g. `make PREFIX=/usr install` you should run `pkgmgr install <package name> 1 '' <install command>` 

###### 2. List packages
To list the packages that currently pkgmgr knows of use `pkgmgr list` 

###### 3. List files of a package
To list the files installed by a package, use `pkgmgr listfiles <package name>`

###### 4. To remove a package
`pkgmgr remove <package name>`

To remove a package permanently:
`pkgmgr premove <package name>`

## Examples
To install PyGTK-2.24.0, 

1. Extract the tarball
2. (Optional) Apply any patches that are required for this package.
3. `cd pygtk-2.24.0`
4. `./configure --prefix=/usr`
5. `make`
6. `sudo pkgmgr install pygtk-2.24.0`

## For the techies:

btrfs has snapshot feature and has facility to list the files created between two marker files in the snapshot. So, this utility creates a starting marker, create thes snapshot/subvolume, installs the package using the provided command (or `make install` by default) and then does `subvolume find-new` to get the list of files installed by the `make install` command. Creates an archives from the file list and extracts it in '/'. It also keeps a INSTALL.log file with the package files containing the details. It keeps the name of the package in flat file under 'metadata' directory.

