# Linux prep-logs
Q | Info 
--- | ---
lsmod| list device drivers
modinfo|info about the kernel driver
libc| Is the 'C' library. it provides system calls.
__zypper in git__|installs git
git clone|mirror remote repository into local system
man 2 intro| introduction to system calls
man 2 syscall| explains syscals
man 2 mmap| haaw to use mmap
man 3 intro|library calls
strace|trace system calls
strace -c  ls|timings for system calls
strace ls 2>&1|grep open| grep standard error
man 7 signal| signal review
check copy progress| dd if=/dev/zero of=/dev/null &
 | kill -s USR1 $(pidof dd)
