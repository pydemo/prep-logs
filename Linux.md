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
Copy nothing to nowhere| dd if=/dev/zero of=/dev/null &
check copy progress| kill -s USR1 $(pidof dd)
system partitions|sudo gdisk /dev/sda
p|systems partitions
boot file|ls /boot/efi/EFI/redhat/grub.efi
see boot sector|sudo xxd -l 512 /dev/sda
xxd|hexadecimal viewwer
Control+A/E| beg/end of line
Alt+F/B |beg/end of a word
Ctrl+K | deletes the line upto the cursor position
Ctrl+W | deletes forward of the word
Esc+F/B| jump forwards and backwards of the word.



