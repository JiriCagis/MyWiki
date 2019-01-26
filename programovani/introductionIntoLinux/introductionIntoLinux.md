# Introduction into Linux

## What is it?
Linux is an open source computer operating system, initially developed on and for Intel x86-based personal computers. It has been subsequently ported to an astoundingly long list of other hardware platforms, from tiny embedded appliances to the world's largest supercomputers.

<div align="center">
	<img src="images/linuxLogo.jpg">
</div>

## History
Linus Torvalds was a student in Helsinki, Finland, in 1991, when he started a project: writing his own operating system kernel. He also collected together and/or developed the other essential ingredients required to construct an entire operating system with his kernel at the center. It wasn't long before this became known as the Linux kernel. 

In 1992, Linux was re-licensed using the General Public License (GPL) by GNU (a project of the Free Software Foundation or FSF, which promotes freely available software), which made it possible to build a worldwide community of developers. By combining the kernel with other system components from the GNU project, numerous other developers created complete systems called Linux distributions in the mid-90’s.

The Linux distributions created in the mid-90s provided the basis for fully free (in the sense of freedom, not zero cost) computing and became a driving force in the open source software movement. In 1998, major companies like IBM and Oracle announced their support for the Linux platform and began major development efforts as well.

Today, Linux powers more than half of the servers on the Internet, the majority of smartphones (via the Android system, which is built on top of Linux), and all of the world’s most powerful supercomputers.

Linus named his OS kernel Linux and shows a penguin as it mascot after a little incident at the zoo.

<div align="center">
	<img src="images/LFS01_ch02_sectiom_01_screen05.jpg"/>
</div>

## Philosophy
Linux is constantly enhanced and maintained by a network of developers from all over the world collaborating over the Internet, with Linus Torvalds at the head. Technical skill, a desire to contribute, and the ability to collaborate with others are the only qualifications for participating.

Linux borrows heavily from the well-established UNIX operating system. It was written to be a free and open source system to be used in place of UNIX, which at the time was designed for computers much more powerful than PCs and was quite expensive. Files are stored in a hierarchical filesystem, with the top node of the system being the root or simply "/". Whenever possible, Linux makes its components available via files or objects that look like files. Processes, devices, and network sockets are all represented by file-like objects, and can often be worked with using the same utilities used for regular files. Linux is a fully multitasking (i.e. multiple threads of execution are performed simultaneously), multiuser operating system, with built-in networking and service processes known as daemons in the UNIX world.

__Note__: Linux was inspired by UNIX, but it is not UNIX.

## Terminology
**Kernel**: glue between hardware and applications. It controls the hardware and makes the hardware interact with the applications.

**Distribution**: collection of programs combined with the Linux kernelto make up a Linux-based operating system. For example: Redhat, Fedora, Ubuntu and Gentoo.

**Boot loader**: program that boots the operating system. Two examples of a boot loader are GRUB and ISOLINUX.

**Service**: program runs as a background process, some examples of the service are httpd, ftpd and named.

**Filesystem**: method for storing and organizing files, some examples of filesystems are ext3, ext4 and FAT.

**X Window system**: standard toolkit for build graphical subsystem on nearly all Linux systems (GNOME, KDE, Xfc)

**Command line**: interface for typing commands on top of the operating system

**Shell**: command line interpreter that interprets the command line input and instruct the operating system to perform any necessary tasks and commands.

## Distributions
The Linux kernel is the core of the operating system. A full Linux distribution consists of the kernel plus a number of other software tools for file-related operations, user management, and software package management. 

<div align="center">
	<img src="images/distroroles.png"/>
</div>

Large organizations, such as companies and governmental institutions and other entities, tend to choose the major commercially-supported distributions from Red Hat, SUSE, and Canonical (Ubuntu).

CentOS is a popular free alternative to Red Hat Enterprise Linux (RHEL) and is often used by organizations that are comfortable operating without paid technical support. Ubuntu and Fedora are widely used by developers and are also popular in the educational realm. Scientific Linux is favored by the scientific research community for its compatibility with scientific and mathematical software packages. Both CentOS and Scientific Linux are binary-compatible with RHEL; i.e. in most cases, binary software packages will install properly across the distributions.
