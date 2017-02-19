# Open your EasyBox 904 xDSL for every provider
This is a step by step guide to open your easybox 904 xDSL for the usage with every provider. This Guide is only supported for the 904 xDSL however it should also work with the LTE version or the EasyBox 804 etc.

# Prerequisites:
- EasyBox 904 xDSL
- computer with a Linux distribution (a Raspberry Pi is fine)
- LAN cables
- FAT32 USB stick
- working internet connection

#Step One

Everything in step one is performed on the **Linux computer** with the **terminal**.

Create a working directory with:

`mkdir easyboxhack`
`cd easyboxhack`

Download the **original firmware image** from vodafone:

`wget https://www.vodafone.de/downloadarea/fullimage_AT904X-03.17.01.16.zip`

To extract the zip you may have to install unzip:

`sudo aptitude install unzip`

Then extract the zip with:

`unzip fullimage_AT904X-03.17.01.16.zip`

Install `binwalk`, a program to analyse the firmware binary image:

`sudo aptitude install binwalk`

Install LZMA libraries so squashfs can extract the filesystem:

`sudo aptitude install liblzma-dev lzma-dev`

Download `squashfs`, a utility to extract the filesystem from the firmware:

`wget https://heanet.dl.sourceforge.net/project/squashfs/squashfs/squashfs4.3/squashfs4.3.tar.gz`

Extract the tar with:

`tar -xzf squashfs4.3.tar.gz`

Install the compilers for squashfs:

`sudo aptitude install gcc g++`

Alter the Makefile of squashfs to include LZMA support:

`cd squashfs4.3/squashfs-tools`
`nano Makefile`

Scroll to the line with "LZMA_XZ_SUPPORT = 1" and delete the `#`on the beginning to uncomment it. Then compile squashfs with:

`make`

When the compiler was successful install with:

`sudo make install`

If you never created a ssh-key you need to do it with (you can leave everything at default):

`ssh-keygen`

Save an empty file named: "sesame.txt" on the root of your FAT32 formated USB-stick. This file will initialize the ssh access.

To check if the step one was succesful you should check this list:

+ Type `unsquashfs` a list of options should be displayed.
+ Type `binwalk` a list of option should be displayed.
+ Check if there is a file named "fullimage_AT904X-03.17.01.16.bin" in your easyboxhack directory.
+ There should be a empty file on your thumb drive named "sesame.txt". Now put it aside we will need it later.


#Step two

After setting up everything we will analyse the firmware image and extract the filesystem to add the ssh key etc.

Inside of your `easyboxhack` directory type:

`binwalk fullimage_AT904X-03.17.01.16.bin`

The output should look like this:

![binwalk](https://github.com/majuss/easybox904/blob/master/static/binwalk_out.png)
