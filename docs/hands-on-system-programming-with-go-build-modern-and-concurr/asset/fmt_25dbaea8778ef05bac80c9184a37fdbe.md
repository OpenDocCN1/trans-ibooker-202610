
## Loading and Saving Settings

Loading will try to scan the content of the file (if present) as a duration and change the settings value.

Saving will write the duration to the file, and it will use its string format. The following code depicts this:

```go
func changeSettings(d *time.Duration, v time.Duration) {
    *d = v
    log.Println("Changed", v)
}

func loadSettings(d *time.Duration) error {
    b, err := ioutil.ReadFile(cfgPath)
    if err != nil {
        return err
    }
    var v time.Duration
    if v, err = time.ParseDuration(string(b)); err != nil {
        return err
    }
    *d = v
    log.Println("Loaded", v)
    return nil
}

func saveSettings(d *time.Duration) error {
    f, err := os.OpenFile(cfgPath,
        os.O_WRONLY|os.O_CREATE|os.O_TRUNC, 0644)
    if err != nil {
        return err
    }
    defer f.Close()

    if _, err = fmt.Fprint(f, d); err != nil {
        return err
    }
    log.Println("Saved", *d)
    return nil
}
```

We will get the path of the user's home directory in the `init` function and use it to compose the path of the `settings` file, as follows:

```go
var cfgPath string

func init() {
    u, err := user.Current()
    if err != nil {
        log.Fatalln("user:", err)
    }
    cfgPath = filepath.Join(u.HomeDir, ".multi")
}
```

We can launch the application in a Terminal and use another Terminal to send signals, as follows:

| **Terminal 1** | **Terminal 2** |
| --- | --- |
| ```bash\n$ go run ch08/signal/multi/multi.go\nLoaded 1s\nAfter 1s Executing action!\n\nChanged 2s\nAfter 2s Executing action!\n\nChanged 4s\nAfter 4s Executing action!\n\nChanged 2s\nAfter 2s Executing action!\n\nSaved 1s\n\n$\n``` | ```bash\n$ kill -SIGUSR1 $(pgrep multi)\n\n$ kill -SIGUSR1 $(pgrep multi)\n\n$ kill -SIGUSR2 $(pgrep multi)\n\n$ kill -SIGINT $(pgrep multi)\n``` |

In the left column, we can see the output from the application; in the right column, we can see the commands we launched to trigger them. In order to get the PID of the running application, we are using the `pgrep` command and nesting in `kill`.

## Sending Signals to Other Processes

After seeing the way in which incoming signals are handled, let's take a look at how to send signals to other processes programmatically. The `os.Process` structure is the only tool we need—its `Signal` method makes it possible to send a signal to the project. It's that easy!

The less simple part is obtaining the process. There are two use cases, as follows:

*   The process is a child, and we have the process value already as a result of `os.StartProcess` or `exec.Command` structs.
*   The process is already existing but we do not have it, so we need to search for it using its PID.

The first use case is simpler, as we already have the process as a variable, or as an attribute of an `exec.Cmd` variable, and we can call the method directly.

The other use case requires searching for the process by PID using the `os.FindProcess` method, as follows:

```go
p, err := os.FindProcess(pid)
if err != nil {
    panic(err)
}
```

Once we have `os.Process`, we can use its `Signal` method to send a specific signal to it, as follows:

```go
if err = p.Signal(syscall.SIGTERM); err != nil {
    panic(err)
}
```

The type of signal we will send to a process depends on the destination process and the behavior we want to suggest, such as an interruption or a termination.

## Connecting Streams

Streams in Go are an abstraction that makes it possible to see any sort of communication or data flow as a series of readers and writers. We have already learned that streams are an important element of Go. We'll now learn how to use what we already know about input and output to control the streams related to a process—the input, output, and error.

### Pipes

Pipes are one of the best ways of connecting input and output in a synchronous way, allowing processes to communicate.

#### Anonymous Pipes

Different commands can be chained in a sequence when using a shell, making one command output the following command input. For example, consider the following command:

```bash
cat book_list.txt | grep "Game" | wc -l
```

Here, we are displaying a file, using the preceding command to filter rows containing a certain string and, finally, using the filtered output to count the lines.

This can be done programmatically in Go when the processes are created inside the application.

The `io.Pipe` function returns a reader/writer couple that is connected; whatever gets written to the pipe writer will be read by the pipe reader. The write operation is a blocking one, which means that all of the written data has to be read before a new write operation can be executed.

We already saw that `exec.Cmd` allows generic streams for its output and input—this allows us to use the values returned by the `io.Pipe` function to connect one process to the other.

First, we define the three commands, as follows:

*   `cat` with index `0`
*   `grep` with index `1`
*   `wc` with index `2`

Then, we can define the two pipes we need, as shown in the following code:

```go
r1, w1 := io.Pipe()
r2, w2 := io.Pipe()

var cmds = []*exec.Cmd{
   exec.Command("cat", "book_list.txt"),
   exec.Command("grep", "Game"),
   exec.Command("wc", "-l"),
}
```

Next, we link the input and output streams. We connect the `cat` (command `0`) output and the `grep` (command `1`) input, and we do the same for the `grep` output and the `wc` input:

```go
cmds[1].Stdin, cmds[0].Stdout = r1, w1
cmds[2].Stdin, cmds[1].Stdout = r2, w2
cmds[2].Stdout = os.Stdout
```

We then start our commands, as follows:

```go
for i := range cmds {
    if err := cmds[i].Start(); err != nil {
        log.Fatalln("Start", i, err)
    }
}
```

We wait until the end of the execution of each command and we close the respective pipe writer; otherwise, the next command reader will hang. To make things easier, each pipe writer is an element in a slice, and each writer has the same index as the command that it is linked to. The last one is `nil` because the last command is not linked by a pipe:

```go
for i, closer := range []io.Closer{w1, w2, nil} {
    if err := cmds[i].Wait(); err != nil {
        log.Fatalln("Wait", i, err)
    }
    if closer == nil {
        continue
    }
    if err := closer.Close(); err != nil {
        log.Fatalln("Close", i, err)
    }
}
```

There are also other tools provided by the `io` package that can help simplify some operations.

#### Standard Input and Output Pipes

The `io.MultiWriter` function makes it possible to write the same content to several readers. This will be extremely useful when the output of a command needs to be broadcast to a series of different commands automatically.

Let's say that we want to do what we did before (that is, look for a word in a file), but for different words. We can use the `MultiWriter` function to copy the output to a series of `grep` commands, each of which will be connected to its own `wc` command.

We will use two auxiliary methods of `exec.Command` in this example:

*   `Cmd.StdinPipe`: This returns a `PipeWriter` struct that will be connected to the command standard input.
*   `Cmd.StdoutPipe`: This returns a `PipeReader` struct that will be connected to the command standard output.

Let's start by defining a list of search terms: one for the command's tuples (`grep` and `wc`), one for writers connected to the first command, and one for the final output of each command chain:

```go
var (
    words = []string{"Game", "Feast", "Dragons", "of"}
    cmds = make([][2]*exec.Cmd, len(words))
    writers = make([]io.Writer, len(words))
    buffers = make([]bytes.Buffer, len(words))
    err error
)
```

Now let's define the commands and their connection—each `grep` command will be connected on one side to `cat` using the `MultiWriter` function with the piped writers, and on the other side to the input of the `wc` command:
