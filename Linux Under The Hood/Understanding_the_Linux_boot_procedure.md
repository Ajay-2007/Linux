### Understanding the Linux Boot Procedure

----------------------------------------------------------------
### Understanding BIOS and UEFI

- BIOS and UEFI are interfaces offered to PC platform to access disks
- UEFI (Unified Extensible Firmware Interface) / EFI (Extensible Firmware Interface) 
were introduced to replace BIOS that has been introduced in 1981 and not seen major 
changes since.
- Both BIOS and UEFI were developed to provide a platform that sits between hardware
/firmware and the operating system
- Using UEFI offers several advantages, which is why it is becoming the standard
interface on modern PCs
- The most significant advantage is its ability to boot from large disks when a GUID
Partition Table (GPT) is used

--------------------------------------------------------
### How the boot loader loads the kernel
- The boot loader needs to find the kernel and start it with the appropriate parameters
- The challenge is that the boot loader needs to read the kernel somewhere from disk,
even if it doesn't have any kernel drivers yet
- For this early access, disk firmware provides Linear Block Acess (LBA), which 
provides a slow but uniform method to access disks
- Most boot loaders have minimal filesystem knowledge, allowing them to read the 
kernel file from disk

--------------------------------------------------------------

### Booting from UEFI or BIOS disks
- On a BIOS system, the Master Boot Record (MBR) is read from disk and the stage 1
boot loader is activated
    - The stage 1 boot loader is only 446 bytes and its task is to load the larger
    stage 2 boot loader that resides in the first MB of the disk

- On an UEFI system, a GPU partition is alwasy used; an MBR is maintained for 
backward compatibility
- The GPT partition table contains an EFI System Partition(ESP) which contains a 
directory with the name efi
- In this directory, each boot loader has its own identifier and corresponding sub
directory (/efi/microsoft. /efi/grub)
- The boot loader itself has the .efi extension and resides in these subdirectories,
along with its supporting files

user@ubuntu~:# gdisk /dev/sda
user@ubuntu~:# mount | grep sda
user@ubuntu~:# cd /boot/efi/
user@ubuntu~:# cd EFI
user@ubuntu~:# \ls (doesn't show directory or different file colors)
user@ubuntu~:# xxd -l 512 /dev/sda
user@ubuntu~:# fdisk -l /dev/sda

------------------------------------------------------------------
### From BIOS/UEFI to boot loader

- The boot loader is reponsible for loading the OS kernel
- Also, it provides other options
    - Select a kernel
    - Pass kernel parameters
    - Support other operating systems
    
--------------------------------------------------------------------
user@ubuntu~:# cd /boot/grub2/ <br>
*Important Notice :- Don't modify the content of grub.cfg file* <br>
user@ubuntu~:# cat grub.cfg<br>
user@ubuntu~:# cd /etc/grub.d/
<br> This one we can modify <br>
user@ubuntu~:# vi /etc/default/grub<br>
user@ubuntu~:# grub2-mkconfig -o /boot/grub2/grub.cfg (This is going to overwrite 
the current grub.cfg) <br>