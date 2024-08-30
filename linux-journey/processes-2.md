---
title: "Processes 2"
sidebar:
  order: 11
---

## Process Termination

When a process is ready to terminate,it lets the kernel know why it's terminating with something called a termination status.

Most commonly a status of `0` means that the process succeeded.

The parent process has to also acknlowedge the termination of the child process by using the wait system call.

### Orphan Processses

When a parent process dies before a child process, the kernel know that it's not going to get a wait call, so instead these it makes these processes "orphans" and puts them under the care of **init** (mother of all processes).

### Zombie Processes

If the child process terminates and the parent processs hasn't called wait yet, the kernel process is turned into a zombie process.

The resources of a child process are still freed up, however there is still an entry in the processs tavle for the zombie process.

Eventually, if the parent process calls the wait system call, the zombie will disappear, this is known as "reaping"

## Signals

A signal is a notification to a process that something has happened.

When a signal is delivered, a process can do a multitude of things:

- Ignore the signal
- "Catch" the signal and perform a specific handler routine
- Process can be terminated, as opposed to the normal exit system call
- Block the signal, depending on the signal mask

### Common signals

Each signal is defined by integers with symbolic names that are in the form of **SIGxxx**

- SIGHUP or HUP or 1: Hangup
- SIGINT or INT or 2: Interrupt
- SIGKILL OR KILL or 9: Kill
- SIGSEGV or SEGV or 11: Segmentation fault
- SIGTERM or TERM or 15: Software termination
- SIGSTOP or STOP: Stop

> Some signals are unblockable, for example the SIGKILL signal. The KILL signal destroys the process.

## kill (Terminate)

```bash
kill 12445
# Here 12445 is the PID of the process you want to kill
# By default kill sends a TERM signal

kill -9 12445
# We can specifiy a signal, here we run a SIGKILL signal
```

- SIGHUP - Hangup, send to a process when the controlling terminal is closed.
  - For example, if you closed a terminal window that had a process running in it, you would get a SIGHUP signal.
- SIGINT - Is an interrupt signal, so you can use <kbd>CTRL</kbd>+<kbd>C</kbd> and the system will try to gracefully kill the process.
- SIGTERM - Kill the process, but allow it to do some cleanup first
- SIGKILL - Kill th eprocess, kill it with firre, doesn't do any cleanup
- SIGSTOP - Stop/suspend a process

## niceness

Processes use the CPU for a small amount of time called a time slice. Then they pause for milliseconds and another process gets a little time slice.

The kernel handles all of these switching of processes and it does a pretty good job at it most of the time.

There is a way to influence the kernel's process scheduling algorithm with a nice value.

Niceness means that a process has a number to determine their priority for the CPU.

- A high number means that the process is nice and has a lower priority for the CPU
- A low or negative number means the process is not very nice and it wants to get as muhc of the CPU as possible.

```bash
top
# NI column shows the processes' niceness level
```

```bash
nice -n 5 apt upgrade
# set priority for a new process
```

```bash
renice 10 -p 3245
# set the priority on an existing process
```

## Process State

```bash
ps aux
```

In the **STAT** column, you'll see lots of values.

- R: running or runnable, it is just waiting for the CPU to process it
- S: Interruptible sleep, wiating for an event to complete, such as input from the terminal
- D: Uninterruptible sleep, proceses that cannot be killed or interrupted with a signal, usually to make them go away you have to reboot or fix the issue.
- Z: Zombie, terminated processes that are waiting to have their statuses collected
- T: Stopped, a process that has been suspended/stopped

## /proc filesystem

_Remember_: everything in Linux is a file, even processes.

Process information is stored in a special filesystem known as the `/proc` filesystem.

```bash
ls /proc
```

There are sub-directories for every **PID**. If you looked at a **PID** in the ps output, you would be able to find it in the `/proc` directory.

```bash
cat /proc/12345/status
```

This will show process state information as well as more detailed information.

The `/proc` directory is how the kernel views the system, so there is alot more information here than what you would see in ps.

## Job Control

### Sending a job to the background

Appending an ampersand (`&`) to the command will run it in the background so you can still use your shell.

```bash
sleep 1000 &
sleep 1001 &
sleep 1002 &
```

### View all background jobs

```bash
jobs
```

```
[1]   Running                 sleep 1000 &
[2]-  Running                 sleep 1001 &
[3]+  Running                 sleep 1002 &
```

This will show you the **job id** in the first column, then the status and the command that was run. The **+** next to the **job ID** means that it is the most recent background job that started. The job with the **-** is the second most recent command.

### Sending a job ot the background on existing job

```bash
sleep 1003
# we can suspend this job running in the foreground with CTRL + Z
```

```bash
bg
# send the command to the background
```

```
# [1]+ sleep 1003 &
```

```bash
jobs
```

```
[1]+  Running                 sleep 1003 &
```

### Moving a job from the background to the foreground

To move a job out of the background just specify the job ID you want.

If you run `fg` without any options, it will bring back the most recent background job (the job with the **+** sign next to it)

```bash
fg %1
# bring job with ID 1 to the foreground
```

### Kill background jobs

```bash
kill %1
# kill job with ID 1
```
