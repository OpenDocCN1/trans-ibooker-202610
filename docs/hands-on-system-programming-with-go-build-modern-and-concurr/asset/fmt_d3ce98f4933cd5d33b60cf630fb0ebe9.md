
A session is a collection of process groups that allow us to impose a series of restrictions on process groups and other operations. A session doesn't allow process group migration to another session, and it prevents processes from creating process groups in different sessions. The `setsid` system call allows us to change the process session to a new session if the process isn't a process group leader. Also, the first process group ID sets the session ID. If this ID is the same as the one of the running process, that process is called the `session leader`.

Now that we've explained these two properties, we can look at the standard operations that are needed to create a daemon, which are usually the following:

*   Clean up the environment to remove unnecessary variables.
*   Create a fork so that the main process can terminate the process normally.
*   Use the `setsid` system call, which accomplishes these three steps:
    1.  Remove the `PPID` from the forked process so that it gets adopted by the `init` process.
    2.  Create a new session for the fork, which will become the `session leader`.
    3.  Set the process as the group leader.
*   The fork's current directory is set to the root directory to avoid having other directories in use, and all the files opened by the parent are closed (the child will open them if needed).
*   Setting standard input to `/dev/null` and using some log files as standard output and error.
*   Optionally, the fork can be forked again, and can then exit. The first fork will be the group leader, and the second one will have the same group, allowing us to have another fork that isn't a group leader.

This is valid for Unix-based operating systems, though Windows also has support for permanent background processes, which are called `services`. Services can start automatically at boot or be started and stopped manually using a visual application called `Service Control Manager` (`SCM`). They can also be controlled from a command line, using the `sc` command in the regular prompt, and via the `Start-Service` and `Stop-Service` cmdlets in PowerShell.

# Daemons in action

Now that we understand what a daemon is and how it works, we can attempt to use the Go standard library to create one. Go applications are multithreaded and don't allow us to call the `fork` system call directly.

We have learned that the `Cmd.Start` method in the `os/exec` package allows us to start a process asynchronously. The second step is to use the `release` method to close all the resources from the current process.

The following example shows us how to do this:

```
package main

import (
    "fmt"
    "os"
    "os/exec"
    "time"
)

var pid = os.Getpid()

func main() {
    fmt.Printf("[%d] Start\n", pid)
    fmt.Printf("[%d] PPID: %d\n", pid, os.Getppid())
    defer fmt.Printf("[%d] Exit\n\n", pid)
    if len(os.Args) != 1 {
        runDaemon()
        return
    }
    if err := forkProcess(); err != nil {
        fmt.Printf("[%d] Fork error: %s\n", pid, err)
        return
    }
    if err := releaseResources(); err != nil {
        fmt.Printf("[%d] Release error: %s\n", pid, err)
        return
    }
}
```

Let's see what the `forkProcess` function does, create another process, and start it:

1.  First, the process working directory gets set to root, and the output and error streams are set to the standard ones:

```
func forkProcess() error {
    cmd := exec.Command(os.Args[0], "daemon")
    cmd.Stdout, cmd.Stderr, cmd.Dir = os.Stdout, os.Stderr, "/"
    return cmd.Start()
}
```

2.  Then, we can release the resources – first, though, we need to find the current process. Then, we can call the `os.Process` method, `Release`, to ensure that the main process releases its resources:

```
func releaseResources() error {
    p, err := os.FindProcess(pid)
    if err != nil {
        return err
    }
    return p.Release()
}
```

3.  The `main` function will contain the daemon logic, which in this example is very simple – it will just print what is running every few seconds:

```
func runDaemon() {
    for {
        fmt.Printf("[%d] Daemon mode\n", pid)
        time.Sleep(time.Second * 10)
    }
}
```

# Services

We have already seen how the first process that lives from boot to the shutdown of the OS is called `init` or `init.d` since it's a daemon. This process is responsible for handling the other daemons, and stores its configuration in the `/etc/init.d` directory.

Each Linux distribution uses its own version of the daemon control process, like `upstart` for Chrome OS or `systemd` in Arch Linux. They all serve the same purpose and have similar behavior.

Each daemon has a control script or application that resides inside `/etc/init.d` and should be able to interpret a series of commands as first arguments, such as `status`, `start`, `stop`, and `restart`. In most cases, the `init.d` file is a script that executes a switch on the argument and behaves accordingly.

# Creating a service

Some applications are capable of automatically handling their service file, and this is what we will try to achieve, step by step. Let's start with an `init.d` script:

```
#!/bin/sh

"/path/to/mydaemon" $1
```

This is a sample script that passes the first argument to the daemon. The path to the binary will be dependent on where the file is located. This needs to be defined at runtime:

```
// ErrSudo is an error that suggest to execute the command as super user
// It will be used with the functions that fail because of permissions
var ErrSudo error

var (
    bin string
    cmd string
)

func init() {
    p, err := filepath.Abs(filepath.Dir(os.Args[0]))
    if err != nil {
        panic(err)
    }
    bin = p
    if len(os.Args) != 1 {
        cmd = os.Args[1]
    }
    ErrSudo = fmt.Errorf("try `sudo %s %s`", bin, cmd)
}
```

The `main` function will handle the different commands, as follows:

```
func main() {
    var err error
    switch cmd {
    case "run":
        err = runApp()
    case "install":
        err = installApp()
    case "uninstall":
        err = uninstallApp()
    case "status":
        err = statusApp()
    case "start":
        err = startApp()
    case "stop":
        err = stopApp()
    default:
        helpApp()
    }
    if err != nil {
        fmt.Println(cmd, "error:", err)
    }
}
```

How can we make sure that our app is running? A very sound strategy is to use a `PID` file, which is a text file that contains the current PID of the running process. Let's define a couple of auxiliary functions to achieve this:

```
const (
    varDir = "/var/mydaemon/"
    pidFile = "mydaemon.pid"
)

func writePid(pid int) (err error) {
    f, err := os.OpenFile(filepath.Join(varDir, pidFile), os.O_CREATE|os.O_WRONLY, 0644)
    if err != nil {
        return err
    }
    defer f.Close()
    if _, err = fmt.Fprintf(f, "%d", pid); err != nil {
        return err
    }
    return nil
}

func getPid() (pid int, err error) {
    b, err := ioutil.ReadFile(filepath.Join(varDir, pidFile))
    if err != nil {
        return 0, err
    }
    if pid, err = strconv.Atoi(string(b)); err != nil {
        return 0, fmt.Errorf("Invalid PID value: %s", string(b))
    }
    return pid, nil
}
```

The `install` and `uninstall` functions will take care of adding or removing the service file located at `/etc/init.d/mydaemon` and requires us to launch the app with root permissions because of the file's location.
