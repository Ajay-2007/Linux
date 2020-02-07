### Understatnding what is involved


If a Linux user performs a simple task, a lot of things needs to happend to acomplish
that task. Have you for example ever wondered what's involved to simply read a file, as
in the `cat /etc/hosts` command?
- The `cat`  command must be read and loaded from disk in RAM
- Related libraries must be found and loaded in RAM also
- The /etc/hosts file needs to be located on disk
- Permissions of the current user need to be checked on this file
- If that is appropriate, the file contents can be copied to RAM

All these different tasks are provided through system calls and library calls.

------------------------------------------------------------------------------
### Understanding System Calls
- Processes can not access the kernel directly
- System calls are used as an interface for processes to the kernel
    - glibc provides a library interface to use system calls from programs
- Common tasks like opening, listing, reading and writing to files all involve system
calls
- The `fork()` and `exec()` system calls determine how a process starts
    - `fork()`: the kernel creates an almost identical copy of the current process and
    replaces that
    - `exec()`: the kernel starts a program, which replaced the current process
    
    -------------------------------------------------------------------------
    ### Reading about System calls in man
    
    user@ubuntu:~$ man man
    user@ubuntu:~$ man 2 intro
    user@ubuntu:~$ man 2 syscall
    
    -------------------------------------------------------------------------
    ### Understanding library calls
    
    - System calls are provided by the kernel to give access to restricted parts
    - Library calls come from shared libraries and provide functionality
    - There's a large number of library calls, virtually unlimited because it all
    depends on the shared libraries that are loaded
    - See also **user@ubuntu:~$ man 3 intro**
    
    ------------------------------------------------------------------------
    
    ### User space vs Kernel space
    - Hardware access is restricted to the kernel only.
    - The kernel provides system calls for users and processes to access hardware
    - User space is memory that is allocated by the kernel for user processes
    - Several elements are all running in user space
        - Network configuration
        - Services like a web server
        - Applications
        - User interfaces
    - Users are created to assign permissions and limits to entitites on Linux
    - Every process or task running on Linux has an owner
    

----------------------------------------------------------------------------
### Using `strace` and `ltrace`

- Use  `strace`  to trace System calls
user@ubuntu:~$ strace ls <br>
user@ubuntu:~$ strace -c ls (prints the count of each syscall occurs in ls)<br>
user@ubuntu:~$ strace ls | grep open (doesn't work cause all the messages that strace shows<br>
is stderr but `|` expects output of first command and input to the second command 
so we have redirect the stderr to stdout)<br>
user@ubuntu:~$ strace ls 2>&1 | grep open<br>

- Use `ltrace` to trace library calls
- user@ubuntu:~$ ltrace ls 2>&1 | less

------------------------------------------------------------------------
### Understanding Signals

- A signal provides a software interrupt; it's a method to tell a process that 
it has to do something
- Signals are strictly defined, see man (7) signal for complete overview
- Some signals such as (sigkill and sigterm) cannot be ignored, others can be ignored
- Many signals are specific to a command
- user@ubuntu:~$ man 7 signal
- user@ubuntu:~$ dd if=/dev/zero of=/dev/null &
- user@ubuntu:~$ kill -s USR1 $(pidof dd)