---
date: 2016-10-14T18:58:40+02:00
title: Installation instructions
weight: 0
---

Just type:
```
$ gem install cangallo
```
Then type
```
$ canga
```
The output should be:
```
$ canga
Commands:
  canga --version, -V        # show version
  canga add FILE [REPO]      # add a new file to the repository
  canga build CANGAFILE      # create a new image using a Cangafile
  canga create FILE [SIZE]   # create a new qcow2 image
  canga del IMAGE            # delete an image from the repository
  canga deltag TAGNAME       # delete a tag
  canga export IMAGE OUTPUT  # export an image to a file
  canga fetch [REPO]         # download the index of the repository
  canga help [COMMAND]       # Describe available commands or one specific command
  canga import IMAGE [REPO]  # import an image from a remote repository
  canga list [REPO]          # list images
  canga overlay IMAGE FILE   # create a new image based on another one
  canga pull NAME            # download an image from a remote repository
  canga show IMAGE           # show information about an image
  canga sign [REPO]          # sign the index file with keybase
  canga tag TAGNAME IMAGE    # add a tag name to an existing image
  canga verify [REPO]        # verify index signature with keybase
```

## Ubuntu 14.04

This distribution comes with old `qemu-img` and `libguestfs` versions.  To install a newer version of libguestfs (1.28) you can add a ppa with these packages:

``` console
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:mpanella/libguestfs
sudo apt-get update
sudo apt-get install -y ruby libguestfs0 libguestfs-tools
```

You can download a newer `qemu-img` version (2.6.2) from https://canga.io/downloads/qemu-img.bz2 and place it in `/usr/bin/qemu-img`:

``` console
sudo mv /usr/bin/qemu-img /usr/bin/qemu-img-2.0.0
sudo curl -o /usr/bin/qemu-img.bz2 https://canga.io/downloads/qemu-img.bz2
sudo bunzip2 /usr/bin/qemu-img.bz2
sudo chmod +x /usr/bin/qemu-img
```


You can find the instructions to install keybase at this URL:

https://keybase.io/docs/the_app/install_linux

Instructions copied here but make sure you check the previous URL for changes in the procedure:

``` console
curl -O https://prerelease.keybase.io/keybase_amd64.deb
sudo dpkg -i keybase_amd64.deb
sudo apt-get install -f
run_keybase
```


## CentOS 7.2

You can add the `qemu-ev` repository to have newer `qemu` packages. Right now they are version 2.3.0 and `qemu-img` is not compatible with Cangallo but it's better to have an updated version.

``` console
sudo yum install centos-release-qemu-ev
sudo yum install libguestfs-tools
```

You can download a newer `qemu-img` version (2.6.2) from https://canga.io/downloads/qemu-img.bz2 and place it in `/usr/bin/qemu-img`:

``` console
sudo mv /usr/bin/qemu-img /usr/bin/qemu-img-2.3.0
sudo curl -o /usr/bin/qemu-img.bz2 https://canga.io/downloads/qemu-img.bz2
sudo bunzip2 /usr/bin/qemu-img.bz2
sudo chmod +x /usr/bin/qemu-img
```

You can find the instructions to install keybase at this URL:

https://keybase.io/docs/the_app/install_linux

Instructions copied here but make sure you check the previous URL for changes in the procedure:

``` console
sudo yum install https://prerelease.keybase.io/keybase_amd64.rpm
run_keybase
```

## Libguestfs troubleshooting

To check if libguestfs is working correctly we can execute this command:

``` console
libguestfs-test-tool
```

If you get the following error you may need to install a kernel. This can happen because you are running it inside a container that does not have a kernel installed:

``` text
supermin: failed to find a suitable kernel (host_cpu=x86_64).

I looked for kernels in /boot and modules in /lib/modules.

If this is a Xen guest, and you only have Xen domU kernels
installed, try installing a fullvirt kernel (only for
supermin use, you shouldn't boot the Xen guest with it).
```

Also make sure that the kernel is readable by the user executing the command, you may get this other error in case it's not readable:

``` text
cp: cannot open '/boot/vmlinuz-3.13.0-100-generic' for reading: Permission denied
```



## Compiling qemu-img

To compile the linked ``qemu-img`` you can use these commands in an Ubuntu 14.04:

``` console
apt-get install build-essential pkg-config libglib2.0-dev
wget http://wiki.qemu-project.org/download/qemu-2.6.2.tar.bz2
tar xvf qemu-2.6.2.tar.bz2
cd qemu-2.6.2
./configure --static --without-pixman --disable-tools --disable-system
make qemu-img
```

