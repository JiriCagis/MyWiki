# Introduction into Linux
I am fan to programming and I wrote this post, because operation system Linux and its command line make much easier life for routine programming. The text does not presuppose a deeper knowledge of the Linux operating system, post start of basic. It is history, basic terminology over the file system and the boot process. The larges part of post is devode to a command line. I description more than 40 most used commands. In the end, I write more advanced things such as streams, planning process, editor vim and user environments.


Table of contents
=================

<!--ts-->
   * [What is Linux?](#whatIsIt)
   * [History](#history)
   * [Philosophy](#philosophy)
   * [Terminology](#terminology)
   * [Distributions](#distributions)
   * [Boot process](#bootProcess)
   * [Linux filesystems](#linuxFilesystems)
   * [Terminal](#terminal)
     * [Linux commands for file system management](#linuxCommandsForFileSystemManagement)
     * [Linux commands for text processing](#linuxCommandsForTextProcessing)
     * [Linux commands for process management](#linuxCommandsForProcessManagement)
     * [Linux commands for BASH and user environment ](#linuxCommandsForBASH)
     * [Linux commands for help / documentation](#linuxCommandsForDocumentation)
     * [Linux commands for network](#linuxCommandsForNetwork)
  * [Package management system](#packageManagementSystem)
  * [Streams](#streams)
  * [Processes](#processes)
  * [Text editors](#textEditors)
  * [User environment](#userEnvironment)
  * [Resources](#resources)
<!--te-->

<a name=whatIsIt></a>
## What is Linux?
Linux is an open source computer operating system, initially developed on and for Intel x86-based personal computers. It has been subsequently ported to an astoundingly long list of other hardware platforms, from tiny embedded appliances to the world's largest supercomputers.

<div align="center">
	<img src="images/linuxLogo.jpg">
</div>

<a name=history></a>
## History
Linus Torvalds was a student in Helsinki, Finland, in 1991, when he started a project: writing his own operating system kernel. He also collected together and/or developed the other essential ingredients required to construct an entire operating system with his kernel at the center. It wasn't long before this became known as the Linux kernel. 

In 1992, Linux was re-licensed using the General Public License (GPL) by GNU (a project of the Free Software Foundation or FSF, which promotes freely available software), which made it possible to build a worldwide community of developers. By combining the kernel with other system components from the GNU project, numerous other developers created complete systems called Linux distributions in the mid-90’s.

The Linux distributions created in the mid-90s provided the basis for fully free (in the sense of freedom, not zero cost) computing and became a driving force in the open source software movement. In 1998, major companies like IBM and Oracle announced their support for the Linux platform and began major development efforts as well.

Today, Linux powers more than half of the servers on the Internet, the majority of smartphones (via the Android system, which is built on top of Linux), and all of the world’s most powerful supercomputers.

Linus named his OS kernel Linux and shows a penguin as it mascot after a little incident at the zoo.

<div align="center">
	<img src="images/LFS01_ch02_sectiom_01_screen05.jpg"/>
</div>

<a name="philosophy"></a>
## Philosophy
Linux is constantly enhanced and maintained by a network of developers from all over the world collaborating over the Internet, with Linus Torvalds at the head. Technical skill, a desire to contribute, and the ability to collaborate with others are the only qualifications for participating.

Linux borrows heavily from the well-established UNIX operating system. It was written to be a free and open source system to be used in place of UNIX, which at the time was designed for computers much more powerful than PCs and was quite expensive. Files are stored in a hierarchical filesystem, with the top node of the system being the root or simply "/". Whenever possible, Linux makes its components available via files or objects that look like files. Processes, devices, and network sockets are all represented by file-like objects, and can often be worked with using the same utilities used for regular files. Linux is a fully multitasking (i.e. multiple threads of execution are performed simultaneously), multiuser operating system, with built-in networking and service processes known as daemons in the UNIX world.

__Note__: Linux was inspired by UNIX, but it is not UNIX.

<!-- break page -->
<div style="page-break-after: always;"></div>

<a name="terminology"></a>
## Terminology
**Kernel**: glue between hardware and applications. It controls the hardware and makes the hardware interact with the applications.

**Distribution**: collection of programs combined with the Linux kernelto make up a Linux-based operating system. For example: Redhat, Fedora, Ubuntu and Gentoo.

**Boot loader**: program that boots the operating system. Two examples of a boot loader are GRUB and ISOLINUX.

**Service**: program runs as a background process, some examples of the service are httpd, ftpd and named.

**Filesystem**: method for storing and organizing files, some examples of filesystems are ext3, ext4 and FAT.

**X Window system**: standard toolkit for build graphical subsystem on nearly all Linux systems.

**Desktop environment**: graphical user interface on top of the operating system (GNOME, KDE, Xfce)

**Command line**: interface for typing commands on top of the operating system

**Shell**: command line interpreter that interprets the command line input and instruct the operating system to perform any necessary tasks and commands.

<a name="distributions"></a>
## Distributions
The Linux kernel is the core of the operating system. A full Linux distribution consists of the kernel plus a number of other software tools for file-related operations, user management, and software package management. 

<div align="center">
	<img src="images/distroroles.png" width="70%"/>
</div>

Large organizations, such as companies and governmental institutions and other entities, tend to choose the major commercially-supported distributions from Red Hat, SUSE, and Canonical (Ubuntu).

CentOS is a popular free alternative to Red Hat Enterprise Linux (RHEL) and is often used by organizations that are comfortable operating without paid technical support. Ubuntu and Fedora are widely used by developers and are also popular in the educational realm. Scientific Linux is favored by the scientific research community for its compatibility with scientific and mathematical software packages. Both CentOS and Scientific Linux are binary-compatible with RHEL; i.e. in most cases, binary software packages will install properly across the distributions.

### Base distributions
* Debian (extends Ubuntu, Lubuntu)
* Fedora
* Suse

### Questions to ask when choosing a distributions
* What is the main function of the system (server or desktop)?
* What types of packages are important to the organization? For example, web server, word processing, etc.
* How much hard disk space is required and how much is available? For example, when installing Linux on an embedded device, space is usually constrained.
* How often are packages updated?
* How long is the support cycle for each release? For example, LTS releases have long-term support.
* Do you need kernel customization from the vendor or a third party?
* What hardware are you running on? For example, it might be X86, ARM, PPC, etc.
* Do you need long-term stability? Can you accept (or need) a more volatile cutting edge system running the latest software?

<a name="bootProcess"></a>
## Boot process
The Linux boot process is the procedure for initializing the system. It consists of everything that happens from when the computer power is first switched on until the user interface is fully operational. 

1. **First step - BIOS**
	* Starting an x86-based Linux system involves a number of steps. When the computer is powered on, the Basic Input/Output System (BIOS) initializes the hardware, including the screen and keyboard, and tests the main memory. This process is also called POST (Power On Self Test).

	* The BIOS software is stored on a ROM chip on the motherboard. After this, the remainder of the boot process is controlled by the operating system (OS).

1. **Master Boot Record (MBR) and Boot Loader**
	* Master Boot Record Once the POST is completed, the system control passes from the BIOS to the boot loader. The boot loader is usually stored on one of the hard disks in the system, either in the boot sector (for traditional BIOS/MBR systems) or the EFI partition (for more recent (Unified) Extensible Firmware Interface or EFI/UEFI systems). For Linux exists many bootloaders as GRUB, ISOLINUX or DAS.

1. **Load kernel**
	* The boot loader loads the selected kernel image and passes control to it. Kernels are almost always compressed, so its first job is to uncompress itself. After this, it will check and analyze the system hardware and initialize any hardware device drivers built into the kernel.


1. **Init RAM disk**
	* The initramfs filesystem image contains programs and binary files that perform all actions needed to mount the proper root filesystem, like providing kernel functionality for the needed filesystem and device drivers.

3. **Text mode login**
	* Near the end of the boot process, init starts a number of text-mode login prompts. These enable you to type your username, followed by your password, and to eventually get a command shell. However, if you are running a system with a graphical login interface, you will not see these at first.

<div align="center">
	<img src="images/linuxKernel.jpg"/>
</div>

<a name="linuxFilesystems"></a>
## Linux filesystems
A filesystem is a method of storing/finding files on a hard disk (usually in a partition). 

Think of a refrigerator that has multiple shelves that can be used for storing various items. These shelves help you organize the grocery items by shape, size, type, etc. The same concept applies to a filesystem, which is the embodiment of a method of storing and organizing arbitrary collections of data in a human-usable form.

**Filesystems supported by Linux:**

* Conventional disk filesystems: ext2, ext3, ext4, XFS, Btrfs, JFS, NTFS, etc.
* Flash storage filesystems: ubifs, JFFS2, YAFFS, etc.
* Database filesystems
* Special purpose filesystems: procfs, sysfs, tmpfs, squashfs, debugfs, etc.

### The Filesystem Hierarchy Standard

Linux systems store their important files according to a standard layout called the Filesystem Hierarchy Standard (FHS), which has long been maintained by The Linux Foundation. Having a standard is designed to ensure that users, administrators, and developers can move between distributions without having to re-learn how the system is organized.

Linux uses the ‘/’ character to separate paths (unlike Windows, which uses ‘\’), and does not have drive letters. Multiple drives and/or partitions are mounted as directories in the single filesystem.

**Home**: each user has a home directory, usually placed under /home. Home directory contains some configuration files as .bash_profile for setting shell before login, .vimrc file for configure application Vim and etc.

**Bin**: directory contains executable binaries, essential commands used to boot the system or in single-user mode, and essential commands required by all system users, such as: ps, ls, grep.

**Sbin**: directory is intended for essential binaries related to system administration, such as fsck and shutdown.

**Proc**: directory contains virtual files(files exist only in memory) viewing configuration information.It does not contain real files, but runtime system information (e.g. system memory, devices mounted, hardware configuration, etc). Some important files in /proc are: cpuinfo, interrupts, meminfo, mounts, partitions, version.

**Dev**: directory contains device nodes, a type of pseudo-file used by most hardware devices, except for network devices.

**Var**: directory contains files represent log files, database, print queues and temporary folder. Temporary folder is deleted after reboot system.

**Etc**: directory is the home for system configuration files. It contains no binary programs, although there are some executable scripts. For example /etc/network, /etc/group or /etc/passwd.

**Boot**: directory contains the few essential files needed to boot the system.

**Lib** folder contains libraries (common code shared by applications and needed for them to run) for the essential programs in /bin and /sbin.

**Media**: directory is compose from removable devices as USB disks, CDs or DVDs.

**Usr**: directory tree contains theoretically non-essential programs and scripts added to system by users.

**Tmp**: temporary files; on some distributions erased across a reboot and/or may actually be a ramdisk in memory.

<div align="center">
	<img src="images/dirtree.jpg" width="70%"/>
</div>

<a name="terminal"></a>
## Terminal
Linux system administrators spend a significant amount of their time at a command line prompt. They often automate and troubleshoot tasks in this text environment. There is a saying, "graphical user interfaces make easy tasks easier, while command line interfaces make difficult tasks possible". Linux relies heavily on the abundance of command line tools. 

The command line interface provides the following advantages:

* No GUI overhead is incurred.
* Virtually any and every task can be accomplished while sitting at the command line.
* You can implement scripts for often-used (or easy-to-forget) tasks and series of procedures.
* You can sign into remote machines anywhere on the Internet.
* You can initiate graphical applications directly from the command line instead of hunting through menus.
* While graphical tools may vary among Linux distributions, the command line interface does not.

Most input lines entered at the shell prompt have three basic elements:

* Command
* Options
* Arguments

The command is the name of the program you are executing. It may be followed by one or more options (or switches) that modify what the command may do. Options usually start with one or two dashes, for example, -p or --print, in order to differentiate them from arguments, which represent what the command operates on.

However, plenty of commands have no options, no arguments, or neither. In addition, other elements (such as setting environment variables) can also appear on the command line when launching a task.

**Terminal emulator**

A terminal emulator program emulates (simulates) a standalone terminal within a window on the desktop. By this, we mean it behaves essentially as if you were logging into the machine at a pure text terminal with no running graphical interface. Most terminal emulator programs support multiple terminal sessions by opening additional tabs or windows.

**Virtual terminals**

Virtual Terminals (VT) are console sessions that use the entire display and keyboard outside of a graphical environment. Such terminals are considered "virtual" because, although there can be multiple active terminals, only one terminal remains visible at a time. A VT is not quite the same as a command line terminal window; you can have many of those visible at once on a graphical desktop.

To switch between VTs, press CTRL-ALT-function key for the VT. For example, press CTRL-ALT-F6 for VT 6. Actually, you only have to press the ALT-F6 key combination if you are in a VT and want to switch to another VT.

**Sudo**

The user with administrative (admin) privileges when required. sudo allows users to run programs using the security privileges of another user, generally root (superuser). The functionality of sudo is similar to that of run as in Windows.

<a name="linuxCommandsForFileSystemManagement"></a>
## Linux commands for file system management

### ls
By default, list the contents of the current directory. If you provide it a path, it will list the contents of that. Useful options to know are -l and -a, a long list format with more information and show hidden (dot) files, respectively.

```
ls -a			# list all files including hidden file starting with '.'
ls -d 			# list directories - with ' */'
ls -l			# list with long format - show permissions
ls -la			# list long format including hidden files
ls -lh			# list long format with readable file size

ls --colored		# colored list [=always/never/auto]
ls -R			# list recursively directory tree
ls -S			# sort by file size
ls -t			# sort by time & date
ls -X			# sort by extension name
```

### cat
If given a single file, prints its contents to the standard output. If you give it more than one file, it will concatenate them, and you can then redirect the output into a new file. Potentially useful is the -n option, which numbers the lines.

```
cat file.txt			# display contents of file
cat file1 file2			# view contents of multiple files in terminal
cat file.txt | less		# display contents of file gradually
cat > myFile.txt		# create a file with cat command
cat -n file.txt			# display line numbers in file

cat -T file.txt			# display tab separated lines in file
cat file1 > file2		# write content of file1 into file2
cat file1 >> file2		# appends content of file1 at end of file2
cat file1 file2 > file3		# redirecting multiple files contain in a single file
cat file1 file2 | sort > file3	# sorting contents of multiple files in a single file
```

### cd
Allows you to go from current directory to specified directory. Calling it without arguments returns you to your home directory. Calling it with two dots (cd ..) returns you to a directory “above” the current one, while calling it with a dash (cd -) returns you to the previous directory, regardless of where it’s located relative to the current one.

```
cd Documents				# change destination into directory Documents defined by relative path
cd home/user01/Documents		# change destination into directory Documents defined by absolute path 
cd ~					# change destination into user home directory
cd /					# change destination into root directory
```

### pwd
Prints your current directory. Useful if your prompt doesn’t contain this information, and especially useful in BASH programming for obtaining a reference to the directory in which you’re executing the code.

```
pwd		# print your current working directory
pwd -L		# print working directory from environment even if it contains symlinks
pwd -I		# print actual physical current working directory by resolving all symbolic links.
```

### mkdir
Create new directories. The most handy switch is -p, which creates the entire specified structure if it doesn’t exist already.

```
mkdir dogs			# create directory with name dogs in current directory
mkdir -p dir1/dir2/dir3		# parent directories (if non-existent) are created
```


### file
Tells you the type of a file. Since files in Linux aren’t under obligation to have extensions for the system to work (not that having extensions always helps), sometimes it’s hard for the user to know what type of file something is, and this little utility solves that problem.

```
file notes.txt			# determine file type
file note1.txt note2.txt	# determine type of multiple files
```

### cp
Copies files and directories. Since it doesn’t copy directories recursively by default, remember to use -r or -a. The latter preserves mode, ownership and time stamp info in addition to recursively copying.

```
cp main.c bak						# copy single file main.c to destination directory bak
cp main.c def.h /home/usr/rapid/			# copy 2 files main.c and def.h to directory rapid
cp *.c bak						# copy all C files in current directory to subdirectory bak
cp src /home/usr/rapid/					# copy directory src to directory rapid
cp -R dev bak						# copy all files and directories in dev recursively to directory bak
cp -f test.c bak					# force file copy
```

### mv
Moves or renames files and directories. Essentially, moving and renaming is one operation – renaming is just “moving” a single file to the same place under a different name.

```
mv names.txt fullnames.txt		# rename file name.txt onto fullnames.txt
mv -i names.txt fullnames.txt		# rename file, but prompts before overwriting
mv -n names.txt fullnames.txt		# rename file, but not overwrite an existing file
```

### rm
Stands for ‘remove‘ as the name suggests rm command is used to delete or remove files and directory in UNIX like operating system. If you are new to Linux then you should be very careful while running rm command because once you delete the files then you can not recover the contents of files and directory. Though there are some tools and commands through which deleted files can be recovered but for that you need expert skills.

```
rm linuxstufff.log		# delete the file
rm -i notes.txt			# delete files interactively
rm -d appdata/			# delete a empty directory
rm -r dbstore/			# deleting a directory recursively
rm -ri dbstore/			# delete the files and sub-directories interactively
rm -f tech.txt 			# deleting files forcefully
rm -f log{1..5}.txt		# delete files by reqular expression
rm -rf dbstore/			# deleting a directory recursively and forcefully
```

### ln
Creates hard or symbolic links between files. 

__Soft Link__:
Linux OS recognizes the data part of this special file as a reference to another file path. The data in the original file can be accessed through the special file, which is called as Soft Link.

__Hard Link__:
With Hard Link, more than one file name reference the same inode number. Once you create a directory, you would see the hidden directories “.” and “..” . In this, “.” directory is hard linked to the current directory and the “..” is hard linked to the parent directory.

```
ln -s /home/chris/src/library.so library.so		# create a symbolic link for a File
ln -s /home/chris/obj objects				# create a symbolic link for a directory
ln src_original.txt dst_link.txt			# create hard link for files
```


### chmod
Change user permissions. This refers to viewing, writing and executing files. A normal user may change permissions for files he owns. Permissions are defined in order "user", "group", and "other".

*	4 stands for "read",
*	2 stands for "write",
* 	1 stands for "execute"
*	0 stands for "no permission."

```
chmod 754 myfile		# set permission on myFile
```

__Note__: Here the digits 7, 5, and 4 each individually represent the permissions for the user, group, and others, each digit is a combination of the numbers 4, 2, 1, and 0.


### chown
The concept of owner and groups for files is fundamental to Linux. Every file is associated with an owner and a group. You can use chown and chgrp commands to change the owner or the group of a particular file or directory.

```
chown root tmpfile			# change the owner of a file
chown :friends tmpfile			# change the group of a file
chown himanshu:friends tmpfile		# change both owner and the group
```


### find
Search the filesystem for files or directories. Find is a very versatile and powerful command, not only because of its searching capabilities, but also because it allows you to execute arbitrary commands on matching (or non-matching, even) files.

```
find . -name file.txt		# find files using name in current directory
find /home -name tecmint.txt	# Find Files Under Home Directory
find /home -iname tecmint.txt	# Find Files Using Name and Ignoring Case
find / -type d -name Tecmint	# Find Directories Using Name
find . -type f -name "*.php"	# Find all PHP Files in Directory
```


### locate
Unlike find, locate searches the updatedb database for file name patterns. This database contains a snapshot of the filesystem. This makes locate very fast, but also unreliable – it cannot tell whether anything changed since the last snapshot.

```
locale file.txt			# find file in precomputed database files
locale -i "file.txt"		# find file by ignore case sensitive in precomputer database
locale "*.html" -n 20		# find just 20 result from your query
locale "*.html" -c		# display the number of matching entries
```

### du
Show file or directory size. Among the more useful options are -h, which converts the reported sizes into a more human-friendly format, -s which gives only a summary instead of the whole listing, and -d which controls the depth of directory recursion.

```
du -sh file.txt		# displays disk usage file.txt with human readable values (KB, MB, GB) 
du -ah			# displays disk usage of all files and folder in current directory
du -cah 		# displays disk usage of all files and folder in current directory with grand total disk space at the last line
```

### df
Show disk usage. The default output is good enough – it lists every filesystem, reports its size and the amount of used and available space – but you might want to tack on -h, which once again provides a more human-friendly report.

```
df -h 	# displays disk usage on all mounted volumes with human readable values (KB, MB, GB)
```

### dd
The dd command copies a file, converting the format of the data in the process, according to the operands specified. Its power comes from the flexibility – you can tell it the exact block size, it can copy around corrupted data, and it isn’t picky about devices – if you want to overwrite your hard drive with zeroes straight from /dev/zero, you’re welcome to do it. It’s also commonly used for creating live USB sticks from hybrid ISO images.

```
dd if=/dev/sda of=~/disk1.img		# create a ISO disc image from the CD in the computer
dd if=/dev/sda of=~/disk1.img		# create an img file of the /dev/sda hard drive
dd if=disk1.img of=/dev/sda		# restore backup image onto hard drive sda
dd if=/dev/zero of=/dev/sdX bs=1M	# securely erase a hard drive with zeroes
```


### mount / unmount
This pair takes care of mounting and unmounting filesystems. This can range from USB sticks to ISO images. Usually only root has mounting privileges.

```
mount /dev/sda5 /home/xp 	# mount sda5 drive on directory xp
umount /home/xp			# unmount drive sda5 from directory xp
```

<a name="linuxCommandsForTextProcessing"></a>
## Linux commands for text processing

### more and less
These two similar utilities allow you to view text chunked into screenfuls. Imagine a very long output from some command. Perhaps you called cat on a file and your terminal emulator took a few seconds to scroll all the text. Well, if you pipe it into one of these, you can now scroll it at your leisure. Less is newer and offers more options, so there is no reason to use more.

```
more file.txt		# sequentially read the content of file on the screen
cat file.txt | more 	# sequentially read the content of file on the screen
less file.txt		# sequentially read the content of file on the screen
cat file.txt | less 	# sequentially read the content of file on the screen
```	

### head and tail
Another pair, but here both halves have their uses. Head outputs a number of the first (“head”) lines of a file, while tail outputs a number of last (“tail”) lines of a file. The default number is ten, but this can be controlled via the -n option. Another useful switch is -f, which is short for “follow”, which continually outputs any appended lines – so, for instance, if you wanted to monitor a log file instead of constantly opening and closing it, you could use “tail -f /path/to/logfile”.

```	
head flavours.txt	# view the first 10 number of lines from given file
head -n 5 flavours.txt 	# view the first N number of lines from given file
head -5 flavours.txt 	# view the first N number of lines from given file

tail flavours.txt	# view the last 10 number of lines from given file
tail -n 5 flavours.txt	# view the last N number of lines from given file
tail -5 flavours.txt	# view the last N number of lines from given file
tail -f perf.log	# view appended lines of file instantly
```	

### grep
Grep, like all good Unix tools, does one thing, but does it well. It searches text for patterns. By default it looks at standard input, but you can specify files to be searched. A pattern can be a normal string or a regular expression. It can print out matching or non-matching lines, and their context. Every time you run a command which spews a lot of information you don’t need, pipe it into grep and let it do its magic.

```	
grep "this" demo_file			# Search for the given string in a single file
grep "string" FILE_PATTERN		# Checking for the given string in multiple files
grep -i "string" FILE			# Case insensitive search in file
grep -v "string" demo_file		# search lines in file does not contains given string
grep -A 3 -i "ex" demo_file		# display N lines after match
grep -B 3 -i "ex" demo_file		# display N lines before match
grep -C 3 -i "ex" demo_file		# display N lines aroung match

# Match regular expression in files
grep "lines.*empty" demo_file

# highlighting result of search
export GREP_OPTIONS='--color=auto' GREP_COLOR='100;8'
```	

### sort
Sorts lines of text by various criteria. Among the more useful, there’s -n, which sorts by the numeric value of a string, and -r, which reverses the output. An example of where this might come in handy is sorting du output – for example, if you wanted to see the files sorted in descending order according to size, you’d combine the two options.

```	
sort file.txt		# display sort the lines in this file alphabetically
sort -r file.txt	# display sort the lines in file revese order alphabetically
sort -n file.txt	# display sort the lines in this file numerically
sort -rn file.txt	# display sort the lines in this file reverse order numerically
```	

### wc
The command line word counting utility. And line counting. And byte counting. And character counting.

```	
wc -l file.txt		# print the newline counts in the file
wc -w file.txt		# print the word counts in the file
wc -m file.txt		# print the character counts in the file
```	


### diff
Shows the difference between two files via line by line comparison. It only shows altered lines, abbreviating changed as c, deleted as d and added as a.

```	
diff file1 file2		# show difference between file1 and file2 line by line
```	

<a name=linuxCommandsForProcessManagement></a>
## Linux commands for process management

### kill / xkill / pkill / killall
All of these serve to “kill” a process, ie terminate it. The difference is what they accept as input. Kill wants the process ID, xkill allows you to click a window to close it, while killall and pkill accept the name of a process, but have somewhat different options and subtly different behavior. Note these do not belong to the same package, and xkill especially is not likely to be installed by default. We advise you to rectify that for your own convenience.

```	
kill 257			# kill process with PID (Process ID) 257
kill 1212 1313 1414		# kill multiple process
kill --TERM -257		# kill process with PID 257 and all child processes
```	


### ps / pgrep
As mentioned, kill needs the process ID. One way to obtain this is by using ps, which prints information about the currently active processes. The default output is not hugely useful, so stick an -e there to see information about every process on the system. This is only a snapshot, it will not update, see top for that. The pgrep command works in the following manner: you give it a process name, it gives you the process ID. Partial matches count, so be careful.

```	
pgrep firefox			# find PID for process named firefox
ps aux | grep [f]irefox		# find PID for process named firefox
```	

### top / htop
These two are similar, both display processes, and can be thought of as console system monitors. We recommend you install htop the first chance you get if your distribution doesn’t ship it by default, as it’s a much improved version of top. For starters, it’s not merely a viewer – it allows you to control processes via its user-friendly console GUI interface.

```	
top	# display and update sorted information about processes
htop	# better formated top command
```	

### time
Time a process. Think of it as a stopwatch for program execution. Useful if you’re curious how much slower is your homework implementation of a sorting algorithm compared to the built-in one. Contrary to what you might expect based on the name, it doesn’t tell you the time. See date for that.

```	
time sort.py input.txt	# display total time execution program
```	

<a name="linuxCommandsForBASH"></a>
## Linux commands for BASH and user environment

### su / sudo
Su and sudo are two ways of accomplishing the same thing – running a command as another user. Depending on what your distribution is, you’ve likely seen only one or the other, but both are serviceable. The difference is that su switches you to a different user, while sudo only runs the command with another user’s privileges.

```	
sudo command 		# run the command with another users privileges
cat /etc/sudoers	# display users have sudo privileges
su			# switch you to the super user or root user
su bob			# switch you to the bob account
```	

### date
Unlike time, date does exactly what you’d expect it – it prints out the date (and time) to the standard output. The output itself can be formatted to your specification, and it takes everything from the usual stuff like year, month, day,
12 or 24 hour format to nanoseconds and the ISO week number. For example, date +”%j %V” would give you the day of the year followed by the ISO week number.

```	
date			# print actuall date time into terminal
```	

### alias
This commands creates or changes aliases to other commands. What this means is, you can give names to new commands (or groupings of commands) or “rename” existing ones. It’s very handy for abbreviating long strings of commands you find yourself using often, or giving more memorable names to things you don’t use that often and have problems memorizing.

```	
alias shortName="your custom command here"
```	

### uname
Outputs some basic system information. By itself, it won’t give you anything very useful (“Linux”), but call it with -a, and it will provide kernel information, as well as tell you the hostname and processor architecture.

```	
uname -a		# print all system information
uname -s		# print the kernel name
uname -n		# print the network node hostname
uname -r		# print kernel release
uname -v 		# print kernel version
uname -m		# print the machine hardware name
uname -p		# print the processor type
uname -i		# print the hardware platform 
```	

### uptime

Tells you how long has the system been running. Not exactly essential information, but good for bragging rights and the occasional compute-things-relative-to-how-long-I’ve-been-at-the-computer situation.

```	
uptime			# print how long the system running
uptime -p		# print how long the system running in pretty format
```	


### sleep

You might be wondering why or how would this ever be useful, but even outside BASH scripts, it has its uses: for example, if you’d like to shutdown the computer after a certain period of time, or even as a makeshift alarm.

```	
sleep 60 | start.sh	# it run start.sh script after 60 seconds sleep
```	


## Linux commands for user management

### useradd, userdel, usermod
These commands allow you do add, delete and modify user accounts. It’s not very likely you’ll be using these often, especially if you’re the sole user of your system, and even if not, you might opt for doing this via a GUI, but it’s good to know what they do and that they’re there in case you suddenly need them.

```	
useradd dog			# create user with name dog
useradd -G admins, dev dog	# create user dog with assigned into many group
userdel dog			# delete user dog and all asociated files
usermod -d /var/www/ dog	# change user home directory
usermod -e 2014-11-01 dog	# set expiration time for user
usermod -G office dog		# add group to existing user
usermod -l dog			# lock user dog

cat /etc/passwd 		# display user account information
cat /etc/shadow			# display secure account information
cat /etc/group			# display group account information
cat /etc/gshadow		# display secure group account information
cat /etc/login.defs		# display shadow password suite configuration
```	
### passwd
As the name suggest passwd command is used to change the password of system users. If the passwd command is executed by non-root user then it will ask for the current password and then set the new password of a user who invoked the command. When this command is executed by super user or root then it can reset the password for any user including root without knowing the current password.

```	
passwd			# change password of current user
passwd dog		# change passwrod of dog user
passwd -S dog		# display password status information

passwd -d dog		# remove password
passwd -e dog		# expire password immediately
passwd -w 10 dog	# set warning days before password expire 
passwd -n 10 dog	# set minimum days to change password
```	

<a name="linuxCommandsForDocumentation"></a>
## Linux commands for help / documentation

### man / whatis
The man command brings up the manual for a particular command. Most command line applications come with a man page. Whatis provides a one line summary lifted from the relevant sections of the manual. What are sections of the manual? See for yourself with “man man”.

```	
whatis	cat		# show short manual for command cat
man cat			# show complete manual for command cat
```	


### whereis
Tells you where an executable binary files lives, provided it’s in your path. It can also find its manual page and source code, provided they are present.

```	
whereis cat		# show source directory of command cat
```	

<a name="linuxCommandsForNetwork"></a>
## Linux commands for network

### ip
The ip command is used to assign an address to a network interface and/or configure network interface parameters on Linux operating systems. This command replaces old good and now deprecated ifconfig command on modern Linux distributions.

```	
ip addr				# display info about all network interfaces
ip -4 addr			# display info about only TCP/IP v.4 network interfaces
ip -6 addr			# display info about only TCP/IP v.6 network interfaces
ip addr show eth0		# display only eth0 interface
ip link ls up			# display only running interfaces

ip a add 192.168.1.200/255.255.255.0 dev eth0	# assign ip address on eth0
ip a del 192.168.1.200/24 dev eth0		# remove ip address from the interface
ip addr add broadcast 172.20.10.255 dev eth0	# assign broadcast address on eth0
ip link set dev eth1 down			# stop running interface eth1
ip link set dev eth1 up				# start interface eth1
```	

### ping
Pings are ICMP ECHO_REQUEST datagrams, but that’s not important. The important thing is that the ping utility is a useful diagnostic tool. It allows you to quickly test if you’re connected to your router or the Internet, and gives some indication of the quality of that connection.

```	
ping www.google.com
ping 216.58.212.78
```	

### wget
Wget is a command line utility that can capably handle the following types of downloads: Large file downloads, Recursive downloads, where a web page refers to other web pages and all are downloaded at once, Password-required downloads or Multiple file downloads.

```	
wget http://website.com/files/file.zip
```	

### curl
Besides downloading, you may want to obtain information about a URL, such as the source code being used. curl can be used from the command line or a script to read such information. curl also allows you to save the contents of a web page to a file, as does wget.

```	
curl http://www.centos.org > centos-org.html
```	

### traceroute
Print the route packets take to network host. The Internet is a large and complex aggregation of network hardware, connected together by gateways.  Tracking the route one's packets follow (or finding the miscreant gateway that's discarding your packets) can be difficult.  traceroute utilizes the IP protocol `time to live' field and attempts to elicit an ICMP TIME_EXCEEDED response from each gateway along
the path to some host. The only mandatory parameter is the destination host name or IP number.

```	
traceroute www.google.cz
```	

<a name="packageManagementSystem"></a>
## Package management system
The core parts of a Linux distribution and most of its add-on software are installed via the Package Management System. Each package contains the files and other instructions needed to make one software component work on the system. Packages can depend on each other. For example, a package for a web-based application written in PHP can depend on the PHP package.

In most Linux distributions have two levels of package management system. Low-level tool (such as dpkg or rpm) takes care of the details of unpacking individual packages, running scripts, getting the software installed correctly, while a high-level tool (such as apt-get, yum, or zypper) works with groups of packages, downloads packages from the vendor, and figures out dependencies. Most of the time users need work only with the high-level tool, which will take care of calling the low-level tool as needed.


**apt**

Advanced packaging tool is the package management system that manages software on Debian-based systems.

```
apt-cache search python     # search available programs with substring python
apt-get install python      # install program python
apt-get remove python       # uninstall program python
apt-get list	 	    # show installed packages on your system
apt-get update              # update your repository with reference on software
apt-get upgrade		    # upgrade operation system onto new version
```
**yum**

Yellowdog Updater Modified is the package management system used for RPM compatible Linux as Fedora.

```
yum search python       # search available programs with substring python
yum install python      # install program python
yum remove python       # uninstall program python
yum list		# show installed packages on your system
yum update              # update your repository with reference on software
```

**zypper**

Zypper is the package management system used for openSUSE distributions.

```
zypper search python       # search available programs with substring python
zypper install python      # install program python
zypper remove python       # uninstall program python
zypper list		   # show installed packages on your system
zypper update              # update your repository with reference on software
```

<a name="streams"></a>
## Streams
### Standard file streams
When commands are executed, by default there are three standard file streams (or descriptors) always open for use: standard input (standard in or stdin), standard output (standard out or stdout) and standard error (or stderr).

Usually, stdin is your keyboard, and stdout and stderr are printed on your terminal. stderr is often redirected to an error logging file, while stdin is supplied by directing input to come from a file or from the output of a previous command through a pipe. stdout is also often redirected into a file. Since stderr is where error messages are written, usually nothing will go there.

In Linux, all open files are represented internally by what are called file descriptors. Simply put, these are represented by numbers starting at zero. stdin is file descriptor 0, stdout is file descriptor 1, and stderr is file descriptor 2. Typically, if other files are opened in addition to these three, which are opened by default, they will start at file descriptor 3 and increase from there.

|Name  			|Symbolic name   |Value   |Example   | 
|----------------|---------------|--------|-----------|
|standard input  |stdin   			|0	      |keyboard  |
|standard output |stdout		   |1  	   |terminal  |
|standard error  |stderr   	   |2		   |log file  |

### I/O redirection
Through the command shell, we can redirect the three standard file streams so that we can get input from either a file or another command, instead of from our keyboard, and we can write output and errors to files or use them to provide input for subsequent commands.

For example, if we have a program called do_something that reads from stdin and writes to stdout and stderr, we can change its input source by using the less-than sign ( < ) followed by the name of the file to be consumed for input data:

```
do_something < input-file
```
If you want to send the output to a file, use the greater-than sign (>) as in:

```
do_something > output-file
```

Because stderr is not the same as stdout, error messages will still be seen on the terminal windows in the above example.

If you want to redirect stderr to a separate file, you use stderr’s file descriptor number (2), the greater-than sign (>), followed by the name of the file you want to hold everything the running command writes to stderr:

```
do_something 2> error-file
```

Note: By the same logic, do_something 1> output-file is the same as do_something > output-file.

A special shorthand notation can send anything written to file descriptor 2 (stderr) to the same place as file descriptor 1 (stdout): 2>&1.

```
do_something > all-output-file 2>&1
```
bash permits an easier syntax for the above:

```
do_something >& all-output-file
```

### Pipes
The UNIX/Linux philosophy is to have many simple and short programs (or commands) cooperate together to produce quite complex results, rather than have one complex program with many possible options and modes of operation. In order to accomplish this, extensive use of pipes is made. You can pipe the output of one command or program into another as its input.

In order to do this, we use the vertical-bar, |, (pipe symbol) between commands as in:

```
command1 | command2 | command3
```

The above represents what we often call a pipeline, and allows Linux to combine the actions of several commands into one. This is extraordinarily efficient because command2 and command3 do not have to wait for the previous pipeline commands to complete before they can begin hacking at the data in their input streams; on multiple CPU or core systems, the available computing power is much better utilized and things get done quicker.

Furthermore, there is no need to save output in (temporary) files between the stages in the pipeline, which saves disk space and reduces reading and writing from disk, which is often the slowest bottleneck in getting something done.

<div align="center">
	<img src="images/pipeline.png" width="70%">
</div>

<a name="processes"></a>
## Processes
### What is process?
A process is simply an instance of one or more related tasks (threads) executing on your computer. It is not the same as a program or a command. A single command may actually start several processes simultaneously. Some processes are independent of each other and others are related. A failure of one process may or may not affect the others running on the system.

Processes use many system resources, such as memory, CPU (central processing unit) cycles, and peripheral devices, such as printers and displays. The operating system (especially the kernel) is responsible for allocating a proper share of these resources to each process and ensuring overall optimized system utilization.

### Process types
|Process type		|Description	|Example|
|----------------|------------|-------|
| Interactive 	|Need to be started by a user, either at a command line or through a graphical interface such as an icon or a menu selection.| bash, firefox, top|
|Batch Processes	|Automatic processes which are scheduled from and then disconnected from the terminal. These tasks are queued and work on a FIFO (first-in, first-out) basis.	|updatedb|
|Daemons |Server processes that run continuously. Many are launched during system startup and then wait for a user or system request indicating that their service is required.|httpd, xinetd, sshd|
|Threads|	Lightweight processes. These are tasks that run under the umbrella of a main process, sharing memory and other resources, but are scheduled and run by the system on an individual basis. An individual thread can end without terminating the whole process and a process can create new threads at any time. Many non-trivial programs are multi-threaded.	|firefox, gnome-terminal-server|
|Kernel Threads|	Kernel tasks that users neither start nor terminate and have little control over. These may perform actions like moving a thread from one CPU to another, or making sure input/output operations to disk are completed.|kthreadd, migration, ksoftirqd|

### Process scheduling and states
A critical kernel function called the scheduler constantly shifts processes on and off the CPU, sharing time according to relative priority, how much time is needed and how much has already been granted to a task.  

When a process is in a so-called running state, it means it is either currently executing instructions on a CPU, or is waiting to be granted a share of time (a time slice) so it can execute. All processes in this state reside on what is called a run queue and on a computer with multiple CPUs, or cores, there is a run queue on each.

<div align="center">
	<img src="images/processScheduling.jpg" width="50%">
</div>

However, sometimes processes go into what is called a sleep state, generally when they are waiting for something to happen before they can resume, perhaps for the user to type something. In this condition, a process is sitting on a wait queue.

There are some other less frequent process states, especially when a process is terminating. Sometimes, a child process completes, but its parent process has not asked about its state. Amusingly, such a process is said to be in a zombie state; it is not really alive, but still shows up in the system's list of processes.

### Process and Thread IDs
At any given time, there are always multiple processes being executed. The operating system keeps track of them by assigning each a unique process ID (PID) number. The PID is used to track process state, CPU usage, memory use, precisely where resources are located in memory, and other characteristics.

New PIDs are usually assigned in ascending order as processes are born. Thus, PID 1 denotes the init process (initialization process), and succeeding processes are gradually assigned higher numbers.

The table explains the PID types and their descriptions:

|ID Type						|Description|
|-------------------------|------------|
|Process ID (PID)			|Unique Process ID number|
|Parent Process ID (PPID)	|Process (Parent) that started this process. If the parent dies, the PPID will refer to an adoptive parent; on recent kernels, this is kthreadd which has PPID=2.|
|Thread ID (TID)				|Thread ID number. This is the same as the PID for single-threaded processes. For a multi-threaded process, each thread shares the same PID, but has a unique TID.|

### User and Group IDs
Many users can access a system simultaneously, and each user can run multiple processes. The operating system identifies the user who starts the process by the Real User ID (RUID) assigned to the user.

The user who determines the access rights for the users is identified by the Effective UID (EUID). The EUID may or may not be the same as the RUID.

Users can be categorized into various groups. Each group is identified by the Real Group ID (RGID). The access rights of the group are determined by the Effective Group ID (EGID). Each user can be a member of one or more groups.

### Priorities
At any given time, many processes are running (i.e. in the run queue) on the system. However, a CPU can actually accommodate only one task at a time, just like a car can have only one driver at a time. Some processes are more important than others, so Linux allows you to set and manipulate process priority. Higher priority processes are granted more time on the CPU.

The priority for a process can be set by specifying a nice value, or niceness, for the process. The lower the nice value, the higher the priority. Low values are assigned to important processes, while high values are assigned to processes that can wait longer. A process with a high nice value simply allows other processes to be executed first. In Linux, a nice value of -20 represents the highest priority and 19 represents the lowest. This does sound kind of backwards, but this convention, the nicer the process, the lower the priority, goes back to the earliest days of UNIX.

### Background and foreground processes
Linux supports background and foreground job processing. A job in this context is just a command launched from a terminal window. Foreground jobs run directly from the shell, and when one foreground job is running, other jobs need to wait for shell access (at least in that terminal window if using the GUI) until it is completed. This is fine when jobs complete quickly. But this can have an adverse effect if the current job is going to take a long time (even several hours) to complete.

In such cases, you can run the job in the background and free the shell for other tasks. The background job will be executed at lower priority, which, in turn, will allow smooth execution of the interactive tasks, and you can type other commands in the terminal window while the background job is running. By default, all jobs are executed in the foreground. You can put a job in the background by suffixing & to the command, for example: updatedb &.

You can either use CTRL-Z to suspend a foreground job or CTRL-C to terminate a foreground job and can always use the bg and fg commands to run a process in the background and foreground, respectively.

### Scheduling future processes using at
Suppose you need to perform a task on a specific day sometime in the future. However, you know you will be away from the machine on that day. How will you perform the task? You can use the at utility program to execute any non-interactive command at a specified time, as illustrated in the diagram:

<div align="center">
	<img src="images/atScheduling.jpg" width="70%">
</div>

### Scheduling future processes using cron
cron is a time-based scheduling utility program. It can launch routine background jobs at specific times and/or days on an on-going basis. cron is driven by a configuration file called /etc/crontab (cron table), which contains the various shell commands that need to be run at the properly scheduled times. There are both system-wide crontab files and individual user-based ones. Each line of a crontab file represents a job, and is composed of a so-called CRON expression, followed by a shell command to execute.

The crontab -e command will open the crontab editor to edit existing jobs or to create new jobs. Each line of the crontab file will contain 6 fields:

|Field		|Description		|Values	
|---------|----------------|---------|
|MIN		|Minutes			|0 to 59	
|HOUR		|Hour field		|0 to 23
|DOM		|Day of Month		|1-31
|MON		|Month field		|1-12
|DOW		|Day Of Week		|0-6 (0 = Sunday)
|CMD		|Command			|Any command to be executed

**Examples:**

The entry * * * * * /usr/local/bin/execute/this/script.sh will schedule a job to execute 'script.sh' every minute of every hour of every day of the month, and every month and every day in the week.

The entry 30 08 10 06 * /home/sysadmin/full-backup will schedule a full-backup at 8.30 a.m., 10-June, irrespective of the day of the week.

### Scheduling future processes using sleep
Sometimes, a command or job must be delayed or suspended. Suppose, for example, an application has read and processed the contents of a data file and then needs to save a report on a backup system. If the backup system is currently busy or not available, the application can be made to sleep (wait) until it can complete its work. Such a delay might be to mount the backup device and prepare it for writing.

sleep suspends execution for at least the specified period of time, which can be given as the number of seconds (the default), minutes, hours, or days. After that time has passed (or an interrupting signal has been received), execution will resume.

The syntax is:

```
sleep NUMBER[SUFFIX]...
```

where SUFFIX may be:

* s for seconds (the default)
* m for minutes
* h for hours
* d for days.

sleep and at are quite different; sleep delays execution for a specific period, while at starts execution at a later time.

<a name="textEditors"></a>
## Text editors
At some point, you will need to manually edit text files. You might be composing an email off-line, writing a script to be used for bash or other command interpreters, altering a system or application configuration file, or developing source code for a programming language such as C or Java.

Linux Administrators quite often sidestep the text editors, by using graphical utilities for creating and modifying system configuration files. However, this can be more laborious than directly using a text editor, and be more limited in capability. Note that word processing applications including those that are part of office suites are not really basic text editors because they add a lot of extra (usually invisible) formatting information that will probably render system administration configuration files unusable for their intended purpose. So, using text editors really is essential in Linux.

### Partition of Linux editors
**gedit** is the default text editor of the GNOME desktop environment and part of the GNOME Core Applications. Designed as a general-purpose text editor, gedit emphasizes simplicity and ease of use, with a clean and simple GUI, according to the philosophy of the GNOME project. It includes tools for editing source code and structured text such as markup languages.

**nano** is a text editor for Unix-like computing systems or operating environments using a command line interface. It emulates the Pico text editor, part of the Pine email client, and also provides additional functionality. Unlike Pico, nano is licensed under the GNU General Public License (GPL). Released as free software by Chris Allegretta in 1999, nano became part of the GNU Project in 2001.

**vi** is a screen editor for Linux. Pronounced (vee-aye), vi stands for visual instrument. It is a widely-used default text editor for Unix-based systems and is shipped with vitually all versions of Unix. It exclusively uses the keyboard and provides a very efficient interface for editing programs and scripts. Vi is somewhat difficult to learn, but programmers are happy to go through the learning curve to gain the provided efficiency. In comparison to a general purpose word processor program, VI is tailored to a more specific profile of usage and users – programmers of UNIX-based systems.

**emacs** is one of the oldest and most versatile text editors available for Linux and UNIX-based systems. It's been around for a long time (more than twenty years for GNU emacs) and is well known for its powerful and rich editing features. Emacs is also more than just a text editor; it can be customized and extended with different "modes", enabling it to be used like an Integrated Development Environment (IDE) for programming languages like Java, C or Python.

<div align="center">
	<img src="images/textEditors.jpg" width="70%">
</div>

### vim
Vim is advanded terminal editor beside Nano and Gedit. It compose with many extra features for programmers as show row numbers, text highlight, searching by a regular expression and auto complete code. On the other hand learning more lenght learning curve from scratch. Vim is a text editor that is upwards compatible to Vi. There are a lot of enhancements above Vi.

```
vim myfile      # Start the vi editor and edit the myfile file
vim -r myfile   # Start vi and edit myfile in recovery mode from a system crash
```

**Vim provides three modes:**

* Command mode: is default after start the editor. Keyboard strokes are interpreted as commands that can modify file contents.
* Insert mode: in this mode you write text into file. Type 'i' to switch to Insert mode from Command mode.
* Line mode: uses line editing commands inherited from older line editors. You type ':' for switch into this mode.

Note: You can press 'Esc' on keyboard for return to base command mode from any mode.
Control operations

**Control operations are call from command mode.**

```
:w      # Write to file
:q      # Quit vim
:q!     # Quit vi even and modifications not save

j       # To move one line down
k       # To move one line up
h       # To move one character left
l       # To move one character right

:n      # Move to line n
/word   # Search for pattern [n - next, N - previous]
u       # Undo previous operation
CTRL+N  # Show possible words for autocomplete

dd      # Delete current line
Ndd     # Delete N lines
yy      # Copy current line
Nyy     # Copy N lines
p       # Paste in current position from clipboard

# Find each occurrence of 'foo' (in all lines), and replace it with 'bar'.
:%s/foo/bar/g

# Find each occurrence of 'foo' (in the current line only), and replace it with 'bar'.
:s/foo/bar/g
```

**Configuration**

The editor has configuration in file .vimrc into home directory given user. In configuration file you can set some setting shown below.

```
set encoding=utf8	" Set utf8 as standard encoding and en_US as the standard language
set number			" Display line numbers
set tabstop=4			" Tab key is 4-spaces-wide

set ignorecase		" Ignore case when searching
set smartcase			" When searching try to be smart about cases
set hlsearch			" Highlight search results

set autoread			" Set to auto read when a file is changed from the outside
syntax enable			" Enable syntax highlighting
set history=100     # By default saves your last 8 commands.
```


<a name="userEnvironment"></a>
## User environment
### Identifying the current user
As you know, Linux is a multi-user operating system, meaning more than one user can log on at the same time.

* To identify the current user, type **whoami**
* To list the currently logged-on users, type **wh**

### User Startup Files
In Linux, the command shell program (generally bash) uses one or more startup files to configure the user environment. Files in the /etc directory define global settings for all users, while initialization files in the user's home directory can include and/or override the global settings.

The startup files can do anything the user would like to do in every command shell, such as:

* Customizing the prompt
* Defining command line shortcuts and aliases
* Setting the default text editor
* Setting the path for where to find executable programs.

### Order of the Startup Files
The standard prescription is that when you first login to Linux, /etc/profile is read and evaluated, after which the following files are searched (if they exist) in the listed order:

~/.bash_profile
~/.bash_login
~/.profile 
where  ~/. denotes the user's home directory. The Linux login shell evaluates whatever startup file that it comes across first and ignores the rest. This means that if it finds ~/.bash_profile, it ignores ~/.bash_login and ~/.profile. Different distributions may use different startup files. 

However, every time you create a new shell, or terminal window, etc., you do not perform a full system login; only a file named ~/.bashrc file is read and evaluated. Although this file is not read and evaluated along with the login shell, most distributions and/or users include the ~/.bashrc file from within one of the three user-owned startup files.

Most commonly, users only fiddle with ~/.bashrc, as it is invoked every time a new command line shell initiates, or another program is launched from a terminal window, while the other files are read and executed only when the user first logs onto the system.

Recent distributions sometimes do not even have .bash_profile or .bash_login (Ubuntu) and some just have it do little more than include .bashrc.

### The root account
The root account is very powerful and has full access to the system. Other operating systems often call this the administrator account; in Linux, it is often called the superuser account. You must be extremely cautious before granting full root access to a user; it is rarely, if ever, justified. External attacks often consist of tricks used to elevate to the root account.

However, you can use the sudo feature to assign more limited privileges to user accounts:

* Only on a temporary basis
* Only for a specific subset of commands.

When assigning elevated privileges, you can use the command su (switch or substitute user) to launch a new shell running as another user (you must type the password of the user you are becoming). Most often, this other user is root, and the new shell allows the use of elevated privileges until it is exited. It is almost always a bad (dangerous for both security and stability) practice to use su to become root. Resulting errors can include deletion of vital files from the system and security breaches.

Granting privileges using sudo is less dangerous and is preferred. By default, sudo must be enabled on a per-user basis. However, some distributions (such as Ubuntu) enable it by default for at least one main user, or give this as an installation option.

### Environment variables
Environment variables are quantities that have specific values which may be utilized by the command shell, such as bash, or other utilities and applications. Some environment variables are given preset values by the system (which can usually be overriden), while others are set directly by the user, either at the command line or within startup and other scripts.

**Home variable**

HOME is an environment variable that represents the home (or login) directory of the user.

```
echo $HOME      # show home directory current user
```


**Path variable**

PATH is an ordered list of directories (the path) which is scanned when a command is given to find the appropriate program or script to run.

```
echo $PATH
```

**PS1**

PS1 is the primary prompt variable which controls what your command line prompt looks like.

```
echo $PS1
    \u - User name
    \h - Host name
    \w - Current working directory
    \! - History number of this command
    \d - Date
```

**Shell variable**

The environment variable SHELL points to the user's default command shell (the program that is handling whatever you type in a command window, usually bash) and contains the full pathname to the shell.

```
echo $SHELL
```

### File permissions
In Linux and other UNIX-based operating systems, every file is associated with a user who is the owner. Every file is also associated with a group (a subset of all users) which has an interest in the file and certain rights, or permissions: read, write, and execute.

|Command		|Usage|
|------------|------|
|chown			|Used to change user ownership of a file or directory
|chgrp			|Used to change group ownership
|chmod			|Used to change the permissions on the file, which can be done separately for owner, group and the rest of the world (often named as other.)

**Order number?**

user: group: other

**What mean single one numbers?**

|Number		| Explain	
|------------|-------------------
|4				|if read permission is desired. 
|2 				|if write permission is desired. 
|1				|if execute permission is desired.
 
Example assign all permissions for user, read and execute for someone else:
```
chmod 755 somefile
```

<a name="resources"></a>
## Resources
* [LFS101x: Introduction to Linux. (Course of university edX)](https://www.edx.org)
* [TecMint (The Ideal Linux Blog for Syssadmins & geeks)](https://www.tecmint.com)
* [Computer Hope (Free computer help since 1998)](https://www.computerhope.com)
* [HowtoForge (Linux tutorials)](https://www.howtoforge.com)
* [Tutorials point](https://www.tutorialspoint.com)







