# Linux Fundamentals on HTB

## Date Tue Dec 12 22:19:03 EST 2023

### Directory Structure

|Path|Description|
|-|-|
|/|The top-level directory is the root filesystem and contains all of the files required to boot the operating system before other filesystems are mounted as well as the files required to boot the other filesystems. After boot, all of the other filesystems are mounted at standard mount points as subdirectories of the root|
|/bin|Contains essential command binaries|
|/boot|Consists of the static bootloader, kernel executable, and files required to boot the Linux OS|
|/dev|Contains device files to facilitate access to every hardware device attached to the system|
|/etc|Local system configuration files. Configuration files for installed applications may be saved here as well|
|/home|Each user on the system has a subdirectory here for storage|
|/lib|Shared library files that are required for system boot|
|/media|External removable media devices such as USB drives are mounted here|
|/mnt|Temporary mount point for regular filesystems|
|/opt|Optional files such as third-party tools can be saved here|
|/root|The home directory for the root user|
|/sbin|This directory contains executables used for system administration (binary system files)|
|/tmp|The operating system and many programs use this directory to store temporary files. This directory is generally cleared upon system boot and may be deleted at other times without any warning|
|/usr|Contains executables, libraries, man files, etc|
|/var|This directory contains variable data files such as log files, email in-boxes, web application related files, cron files, and more|

### Prompt Configuration
|Special Character|Description|
|-|-|
|\d|Date (Mon Feb 6)|
|\D{%Y-%m-%d}|Date (YYYY-MM-DD)|
|\H|Full hostname|
|\j|Number of jobs managed by the shell|
|\n|Newline|
|\r|Carriage return|
|\s|Name of the shell|
|\t|Current time 24-hour (HH:MM:SS)|
|\T|Current time 12-hour (HH:MM:SS)|
|\@|Current time|
|\u|Current username|
|\w|Full path of the current working directory|

### System Commands
|Command|Description|
|-|-|
|whoami|Displays current username.|
|id|Returns users identity|
|hostname|Sets or prints the name of current host system.|
|uname|Prints basic information about the operating system name and system hardware.|
|pwd|Returns working directory name.|
|ifconfig|The ifconfig utility is used to assign or to view an address to a network interface and/or configure network interface parameters.|
|ip|Ip is a utility to show or manipulate routing, network devices, interfaces and tunnels.|
|netstat|Shows network status.|
|ss|Another utility to investigate sockets.|
|ps|Shows process status.|
|who|Displays who is logged in.|
|env|Prints environment or sets and executes command.|
|lsblk|Lists block devices.|
|lsusb|Lists USB devices|
|lsof|Lists opened files.|
|lspci|Lists PCI devices.|

### System Commands Cont.

#### id
- The id command expands on the whoami command and prints out our effective group membership and IDs.
- This can be of interest to penetration testers looking to see what access a user may have and sysadmins looking to audit account permissions and group membership.

#### uname
- Running uname -a will print all information about the machine in a specific order: kernel name, hostname, the kernel release, kernel version, machine hardware name, and operating system.
- The -a flag will omit -p (processor type) and -i (hardware platform) if they are unknown.
- Suppose we want to print out the kernel release to search for potential kernel exploits quickly. We can type uname -r to obtain this information.

#### ls
- -i for for index or "inode"
- -ltr to sort by last modified

### Linux Searching Commands

#### find

- Besides the function to find files and folders, this tool also contains the function to filter the results.
- We can use filter parameters like the size of the file or the date.
- We can also specify if we only search for files or folders.
- Example command
- - find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null

|Option|Description|
|-|-|
|-type f|Hereby, we define the type of the searched object. In this case, 'f' stands for 'file'.|
|-name *.conf|With '-name', we indicate the name of the file we are looking for. The asterisk (*) stands for 'all' files with the '.conf' extension.|
|-user root|This option filters all files whose owner is the root user.|
|-size +20k|We can then filter all the located files and specify that we only want to see the files that are larger than 20 KiB.|
|-newermt 2020-03-03|With this option, we set the date. Only files newer than the specified date will be presented.|
|-exec ls -al {} \;|This option executes the specified command, using the curly brackets as placeholders for each result. The backslash escapes the next character from being interpreted by the shell because otherwise, the semicolon would terminate the command and not reach the redirection.|
|2>/dev/null|This is a STDERR redirection to the 'null device', which we will come back to in the next section. This redirection ensures that no errors are displayed in the terminal. This redirection must not be an option of the 'find' command.|

#### locate

- The command locate offers us a quicker way to search through the system.
- In contrast to the find command, locate works with a local database that contains all information about existing files and folders.
- - The database can be updated by running "sudo updatedb"
- Example command to find all files with .conf extension
-- locate *.conf

