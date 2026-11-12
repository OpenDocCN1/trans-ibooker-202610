
## Listing 2.35: Looking at the /etc/ld.so.conf.d/ file contents on CentOS

```
$ cat /etc/ld.so.conf.d/mariadb-x86_64.conf
/usr/lib64/mysql
$
$ ls /usr/lib64/mysql
libmysqlclient.so.18  libmysqlclient.so.18.0.0  plugin
$
```

## Loading Dynamically

When a program is started, the _dynamic linker_ (also called the _dynamic linker/loader_) is responsible for finding the program’s needed library functions. After they are located, the dynamic linker will copy them into memory and bind them to the program.

Historically, the dynamic linker executable has a name like `ld.so` and `ld-Linux.so*`, but its actual name and location on your Linux distribution may vary. You can employ the `locate` utility (covered in more detail in Chapter 4) to find its actual name and location, as shown in Listing 2.36 on a CentOS distribution.

## Listing 2.36: Locating the dynamic linker executable on CentOS

```
$ locate ld-linux
/usr/lib64/ld-linux-x86-64.so.2
/usr/share/man/man8/ld-linux.8.gz
/usr/share/man/man8/ld-linux.so.8.gz
$
```

When you've located the dynamic linker utility, you can try it out by using it to manually load a program and its libraries (it will run the program as well). An example of this on a CentOS distribution and employing the `echo` utility is shown in Listing 2.37.

## Listing 2.37: Loading and running the echo command with the dynamic linker utility

```
$ /usr/lib64/ld-linux-x86-64.so.2 /usr/bin/echo "Hello World"
Hello World
$
```

Unfortunately in Listing 2.37, you cannot see all the shared libraries the dynamic linker loaded when it initiated the `echo` utility. However, if desired, you can use the `ldd` command to view a program’s needed libraries, and it is covered later in this chapter.

## Library Management Commands

Library directories are not the only resources for managing and troubleshooting application libraries. There are also a few useful tools you can employ. Along with those utilities are a few additional library concepts you should understand.

## Managing the Library Cache

The library cache is a catalog of library directories and all the various libraries contained within them. The system reads this cache file to quickly find needed libraries when it is loading programs. This makes it much faster for loading libraries than searching through all the possible directory locations for a particular required library file.

When new libraries or library directories are added to the system, this library cache file must be updated. However, it is not a simple text file you can just edit. Instead, you have to employ the ldconfig command.

Fortunately, when you are installing software via one of the package managers, the ldconfig command is typically run automatically. Thus, the library cache is updated without any needed intervention from you. Unfortunately, you’ll have to manually run the ldconfig command for any applications you are developing yourself.

## Real World Scenario

### Developing New Libraries

Imagine you’re on an open source development team that is creating a new dynamic function library for a Linux app, which will be offered in your favorite distribution’s repository. The library file is stored in a development directory (/home/devops/library), and it is ready for testing.

To accommodate testing of the newly created program library, you’ll need to modify the LD_LIBRARY_PATH environment variable by including the program in its definition as such:

```
`export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/devops/library/`
```

After testing and refinement of the new function library is completed, move the library file to its production folder (most likely somewhere in the /usr/lib* directory tree). And then create a library configuration file within the /etc/ld.so.conf.d/ directory that points to the library file’s location.

When those items are completed, you’ll need to manually update the library cache. Using super user privileges, issue the ldconfig command to load the new library into the catalog.

If you are troubleshooting the library cache, you can easily see what library files are cataloged by using the ldconfig -v command. An example of this is shown in Listing 2.38. The example command employs a pipe and the grep utility to search for a particular library, as well as redirects any errors into the black hole (these concepts were covered in Chapter 1).

## Listing 2.38: Listing files in the library cache via the ldconfig -v command

```
$ ldconfig -v 2> /dev/null | grep libmysqlclient
    libmysqlclient.so.18 => libmysqlclient.so.18.0.0
$
```

## Troubleshooting Shared Library Dependencies

The ldd utility can come in handy if you need to track down missing library files for an application. It displays a list of the library files required for the specified application. An example is shown using the echo command's file in Listing 2.39.

## Listing 2.39: Using the ldd command to view an application's libraries

```
$ ldd /usr/bin/echo
    linux-vdso.so.1 =>  (0x00007ffd3bd64000)
    libc.so.6 => /lib64/libc.so.6 (0x00007fc7c39eff000)
    /lib64/ld-linux-x86-64.so.2 (0x00007fc7c3a2cc000)
$
```

The ldd utility output shows the echo program requires two external library files: the standard linux-vdso.so.1 and libc.so.6 files. The ldd utility also shows where those files are found on the Linux system, which can be helpful when troubleshooting issues with applications involving their library files.

> Sometimes a library is dependent on another library. So when you are troubleshooting a missing library file, you may need to use the ldd command on the libraries listed for the application in order to get to the root of the problem.

## Managing Processes

Linux must keep track of lots of different programs, all running at the same time. This section covers how Linux keeps track of all the active applications, how you can peek at that information, as well as how to use command-line tools to manage the running programs.

## Examining Process Lists

At any given time lots of active programs are running on the Linux system. Linux calls each running program a process. The Linux system assigns each process a process ID (PID) and manages how the process uses memory and CPU time based on that PID.

When a Linux system first boots, it starts a special process called the init process. The init process is the core of the Linux system; it runs scripts that start all of the other processes running on the system, including the processes that start the text consoles and graphical windows you use to log in (see Chapter 5).

## Viewing Processes with ps

You can look at processes that are currently running on the Linux system by using the `ps` command. The default output of this command is shown in Listing 2.40.

## Listing 2.40: Viewing your processes with the ps command

```
$ ps
PID TTY          TIME CMD
1615 pts/0    00:00:00 bash
1765 pts/0    00:00:00 ps
$
```

By default, the ps program shows only the processes that are running in the current user shell. In this example, we only had the command prompt shell running (Bash) and, of course, the ps command.

The basic output of the ps command shows the PID assigned to each process, the terminal (TTY) that they were started from, and the CPU time that the process has used.

The tricky feature of the ps command (and the reason that makes it so complicated) is that at one time there were two versions of it in Linux. Each version had its own set of command-line options controlling the information it displayed. That made switching between systems somewhat complicated.

The GNU developers decided to merge the two versions into a single ps program, and of course, they added some additional switches of their own. Thus, the current ps program used in Linux supports three different styles of command-line options:

- Unix-style options, which are preceded by a dash
- Berkeley Software Distribution (BSD)-style options, which are not preceded by a dash
- GNU long options, which are preceded by a double dash

This makes for lots of possible switches to use with the ps command. You can consult the ps manual page to see all possible options that are available. Most Linux administrators have their own set of commonly used switches that they remember for extracting pertinent information. For example, if you need to see every process running on the system, use the Unix-style -ef option combination, as shown in Listing 2.41.

## Listing 2.41: Viewing processes with the ps command and Unix-style options

```
$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 10:18 ?        00:00:03 /sbin/init splash
root         2     0  0 10:18 ?        00:00:00 [kthreadd]
root         4     2  0 10:18 ?        00:00:00 [kworker/0:0H]
```
