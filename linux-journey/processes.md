---
title: "Processes"
sidebar:
  order: 10
---

## ps (Processes)

Processes are the programs that are running on your machine.

Each process has an ID associated with it called the **process ID (PID)**

```bash
ps
```

```
PID TTY          TIME CMD
302 pts/0    00:00:00 bash
948 pts/0    00:00:00 ps
```

- PID: Process ID
- TTY: Controlling terminal associated with the process
- TIME: Total CPU usage time
- CMD: Name of executable/command

```bash
ps aux
# a option displays all processes running (including by other users)
# u option shows more details about the processes
# x lists all processes that don't have a TTY associated with it
```

> processes without a TTY associated with it will show a `?` in the TTY field, they are most common in daemon processes that launch as part of the system startup.

```
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.1  0.1 165732 11220 ?        Ss   10:08   0:00 /sbin/init
root         299  0.0  0.0   2496   120 ?        S    10:08   0:00 /init
destocot     302  0.0  0.1  10176  9172 pts/0    Ss   10:08   0:00 -bash
root         305  0.0  0.0   7540  4892 pts/1    Ss   10:08   0:00 /bin/login -f
destocot     392  0.0  0.1   9952  8916 pts/1    S+   10:08   0:00 -bash
destocot    1956  0.0  0.0   7484  3272 pts/0    R+   10:14   0:00 ps aux
```

- USER: the effective user (the one whose access we are using)
- **PID**: Process ID
- %CPU: CPU time used divided by the time the process has been running
- %MEM: Ratio of the process's resident set size to the physical memory on the machine
- VSZ: Virtual memory usage of the entire process
- RSS: Resident set size, the non-swapped physical memory that a task has used
- TTY: Controlling terminal associated with the process
- **STAT**: Process status code
- START: Start time of the process
- TIME: Total CPU usage time
- **COMMAND**: Name of executable/command

### top

The **top** command gives you real time information about the processes running on your system instead of a snapshot.

```bash
top
```

## Controlling Terminal

_re:_ TTY field in the `ps` output is the controlling terminal associated with the process

There are two types of terminals:

- **regular terminal devices**: is a native termianl device that you can type into and send output to your system

- **pseudoterminal devices**: is what we've been working in, they emulate terminals with the shell termianl window and are denoted by PTS. If you look at `ps`, you'll see your shell process under `pts/*`.

Processes are usually bound to a controlling terminal.

There are processes such as daemon processes, which are special processes that are essentially keeping the system running. THis processes run in the background and since we do not want them to be terminated they are not bound to a controlling terminal.

## Process Details

A processs involves a system allocating memory, CPU, I/O to make the program run.

The kernel is in charge of processes, when we run a program the kernel loads up the code of the program in memory, determine and allocates resources and then keeps tabs on each process, it knows:

- The status of the process
- The resources the process is using and receives
- The process owner
- Signal handling

## Process Creation

When a new process is created, an existing process basically clones itself using something called the fork system call.

The fork system call creates a mostly identical child process, this child process takes on a new process ID (PID) and the original process becomes its parent process an dhas something called a parent process ID **PPID**.

```bash
ps l
# the l (long) flag gives a more details view of running processes
```

```
F   UID     PID    PPID PRI  NI    VSZ   RSS WCHAN  STAT TTY        TIME COMMAND
4  1000     302     299  20   0  10176  9176 do_wai Ss   pts/0      0:00 -bash
4  1000     392     305  20   0   9952  8916 core_s S+   pts/1      0:00 -bash
0  1000    3002     302  20   0   7484  1540 -      R+   pts/0      0:00 ps l
```

The **PPID** column is the parent ID. We can see that the **PID** of the bash shell is the **PPID** of the `ps l` command.

When the system boots up the kernel creates a process that is the mother of all processes called **init**, it has a **PID** of 1.
