---
title: "POSIX kill and killall for Windows 7"
date: 2009-08-01 20:37:04
published: true
categories: [sysadmin]
wpid: 526
---

## POSIX

Under unix-like systems, the `kill` command along with `killall` are used regularly to stop a process and tell it to close down. Under these systems you can use `kill` and `killall` to send "signals" to any running process that you have permission to access (usually those which you run under your own login ID). A signal is like an interrupt to the cpu only for processes. There are many types of signal defined by POSIX, but the two common ones used with `kill` and `killall` are `SIGHUP` (hangup), and `SIGKILL` (terminate immediately).

These two signals tell the program to close down neatly and cease operation in the case of `SIGHUP`, and with `SIGKILL` tell the kernel to immediately destroy the process without waiting for the program to tidy up after itself. `SIGHUP` is the default signal sent by `kill` and `killall`, while the `SIGKILL` can be sent via the `-KILL` commandline switch to said commands. `SIGKILL` is useful for when an application is completely unresponsive to input, and needs to be forcibly closed as it won't respond to any `SIGHUP` signals to close cleanly due to unresponsiveness.

The difference between `kill` and `killall` is the processes that they enact the signal upon. `kill` fires the signal at a single process identified by the numeric Process ID which is specified on the commandline. `killall` fires the signal at any and all processes which match the executable name, such as `firefox-bin` in the case of the popular web browser. As with `kill` the executable name is a commandline argument to the `killall` program. Example usage follows:

```bash
kill 12345
```

Kills the process with PID of `12345` by sending the `SIGHUP` to get the program to shut itself down cleanly.

```bash
killall firefox-bin
```

Sends all firefox processes the `SIGHUP` signal to get them all to shutdown cleanly.

```bash
kill -KILL 54321
```

Explicitly terminates the process with ID `54321` without waiting for it to clean up after itself.

```bash
killall -KILL thunderbird-bin
```

Explicitly terminates all thunderbird processes without waiting for them to clean up after themselves.

## Windows 7

In the windows world, the concept of signals isn't so apparent, and so termination of processes can't be done by sending a `SIGHUP` signal to the appropriate image. (Image is the windows term for an executable.) The traditional way to stop a program in Windows is to use the GUI taskmanager. However this is not useful for times when you are logged into a Windows machine via telnet or SSH.

Telnet and SSH are commandline protocols only, and therefore any GUI applications are going to be completely unavailable to the administrator logged into a Windows machine via these methods. Instead, Microsoft has provided the `tasklist` and `taskkill` commandline programs which take the place of POSIX's `p`, `kill` and `killall` programs.

Tasklist dumps a list of all running tasks on the system into the console window (or over the Telnet/SSH link). From this list you can determine the image names and process IDs of any task currently active. Next is the `taskkill` program, which as the name suggests is a way of terminating the execution of a task (or many tasks as we'll explore). Taskkill can take two different methods of identifying the process(es) to be killed. Examples below:

```batch
taskkill /IM firefox.exe
```

Kill all processes with the image name of `firefox.exe`

```batch
taskkill /PID 24680
```

Kill the single process with the PID of `24680`.

Finally, one can append the `/F` switch to the commandline which will change the symantics of how the system will kill the identified process(es). Without the `/F` switch, `taskkill` acts analogously to `kill` or `killall` or more explicitly `kill -HUP` or `killall -HUP`. When the `/F` switch is added to the commandline, taskkill acts more like `kill -KILL` or `killall -KILL`.

## More info

I discovered this tip on [the tweaks.com article about `taskkill`](https://tweaks.com/articles/39559/kill-processes-from-command-prompt/). Their article goes into more depth about the Windows side of things, while I have tried to explain the similarities between the Windows and POSIX paradigms. Be sure to check out [tweaks.com](https://tweaks.com) for more tips and tricks for Windows, and specifically the article linked above for more information about the `taskkill` filter mechanism which allows much more fine-grained control over which process you wish to kill.