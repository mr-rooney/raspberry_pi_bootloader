Raspberry Pi boot loader
========================

This is a snapshot of the official Raspberry Pi boot loader. It is extended by command line
information for using a network file system.


Command line
============

The boot loader needs to pass a command line to the kernel to boot the system properly.
The command line is located in cmdline.txt. However, there are a few additional files
in the format cmdline_XXX.txt that support different settings. Overwrite cmdline.txt
with the required settings.

cmdline_NFS.txt     --> Use NFS as root file system. The network file system is located
                        on a development host machine and the Raspberry Pi loads the required
                        data via a network connection.
                        You need to adopt the (static) IP parameters in the command line to 
                        fit the requirements of your system.
                        e.g. ip=10.0.0.55:10.0.0.80:10.0.0.80:255.255.255.0::eth0:off:10.0.0.80
                                    10.0.0.55 --> is the IP address the Raspberry Pi uses
                                    10.0.0.80 --> Is the server IP address where the NFS is
                                                  located. Change every occurrence of this
                                                  IP address in the command line to the
                                                  appropriate address.
cmdline_SD.txt      --> the root file system is located on the SD card (root partition)

Use cmdline_NFS.txt for development purpose only, use cmdline_SD.txt to use the Raspberry Pi
standalone without any development host machine.


Config
======

You might need to change the configuration in config.txt, especially if you want to use
the UART output on the GPIO header.

Add enable_uart=1 at the end of config.txt to enable console output.



Copying files to the boot partion
=================================

Whenever you compile the kernel, you need to copy the appropriate files onto the SD card,
even if NFS is used. The kernel and the device tree are always loaded from the SD card.
However, it is not necessary to remove the SD card from the Raspberry Pi and use a card
reader to update the proper files on the SD card. As long as the Raspberry Pi uses NFS,
the boot partition of the SD card is mounted in the file system and can therefore be accessed
directly from the host machine.

The kernel image must use the name kernel7.img and is located in the root directory of the 
boot partition.
The device tree must use the name bcm2710-rpi-3-b.dtb and is located in the root directory.
Any device tree overlays must be located in the overlays directory.
The kernel and device tree names might be adopted, if using a different model than
a Raspberry Pi 3 Model B.
 
host> cd [LINUX_KERNEL_DIR]
host> cp arch/arm/boot/zImage [SD_BOOT_PARTITION]/kernel7.img
host> cp arch/arm/boot/dts/*.dtb [SD_BOOT_PARTITION]
host> cp arch/arm/boot/dts/overlays/*.dtbo [SD_BOOT_PARTITION]/overlays