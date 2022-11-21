# Troubleshooting

### Alert - midnight - Web Performance issues

### The process to migrate a web service from server A to server B (ex: web service in Python)

1. Find the code (ls, ps, ...)
2. Move the code to Github
3. Create a CI, use containers if possible
4. Create a CD for dev, stage and prod
5. Deploy in AWS, Lambda, Faregate, GCP using CF/Terraform
6. Create ASG or autoscaling

### What (really) happens when you type ls -l in the shell?

First and foremost, the shell prints the prompt, prompting the user to enter a command. The shell reads the command _**ls -l**_ from the getline() function’s STDIN, parsing the command line into arguments that it is passing to the program it is executing.

The shell checks if _**ls**_ is an alias. If it is, the alias replaces _**ls**_ with its value.

If _**ls**_ isn’t an alias, the shell checks if the word of a command is a built-in.

The environment is copied and passed to the new process

Then, the shell looks for a program file called _**ls** _ where all the executable files are in the system — in the shell’s environment (an array of strings), specifically in the $PATH variable. The $PATH variable is a list of directories the shell searches every time a command is entered. $PATH, one of the environment variables, is parsed using the ‘=’ as a delimiter. Once the $PATH is identified, all the directories in $PATH are tokenized, parsed further using ‘:’ as a delimiter.

The _**ls**_ binary executable file will be located \[in one of the major subdirectories of the **‘/usr’** directory] in the file **‘/usr/bin/ls’** — **‘/usr/bin’** contains most of the executable files (ie. ready-to-run programs).

To execute _**ls**,_ three system calls are made:

> _**fork / execve / wait**_

(System calls are calls to the Kernel code to do _something._)

To create new processes (to run commands) in Unix, the system call _**fork()**_ is made to duplicate the (Shell) parent process, creating a child process of the (Shell) parent process.

The system call _**execve()**_ is made and does three things: the operating system (OS) stops the duplicated process (of the parent), loads up the new program (in this case: _**ls**_), and starts (the new program _**ls**_). _**execve()**_ replaces defining parts of the current process’ memory stack with the new stuff loaded from the _**ls**_ executable file.

the parent process continues to do other things, keeping track of its children as well, using the system call _**wait()**_**.**

If the executable binary (_**ls**_) file does not exist, an error will be printed.\


### What happens when you do ssh google.com?

```
strace ssh localhost
execve("/usr/bin/ssh", ["ssh", "localhost"], 0x7fffafe55c28 /* 23 vars */) = 0
brk(NULL)                               = 0x55dc37df1000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=27376, ...}) = 0
mmap(NULL, 27376, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f6d2b728000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libselinux.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\20b\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=154832, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f6d2b726000
mmap(NULL, 2259152, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d2b2e0000
mprotect(0x7f6d2b305000, 2093056, PROT_NONE) = 0
mmap(0x7f6d2b504000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x24000) = 0x7f6d2b504000
mmap(0x7f6d2b506000, 6352, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f6d2b506000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libcrypto.so.1.0.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\37\6\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=2361984, ...}) = 0
mmap(NULL, 4471264, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d2ae9c000
mprotect(0x7f6d2b0b6000, 2093056, PROT_NONE) = 0
mmap(0x7f6d2b2b5000, 163840, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x219000) = 0x7f6d2b2b5000
mmap(0x7f6d2b2dd000, 10720, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f6d2b2dd000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libdl.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\16\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=14560, ...}) = 0
mmap(NULL, 2109712, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d2ac98000
mprotect(0x7f6d2ac9b000, 2093056, PROT_NONE) = 0
mmap(0x7f6d2ae9a000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0x7f6d2ae9a000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libz.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220\37\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=116960, ...}) = 0
mmap(NULL, 2212016, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d2aa7b000
mprotect(0x7f6d2aa97000, 2093056, PROT_NONE) = 0
mmap(0x7f6d2ac96000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b000) = 0x7f6d2ac96000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libresolv.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\00008\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=101168, ...}) = 0
mmap(NULL, 2206336, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d2a860000
mprotect(0x7f6d2a877000, 2097152, PROT_NONE) = 0
mmap(0x7f6d2aa77000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x17000) = 0x7f6d2aa77000
mmap(0x7f6d2aa79000, 6784, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f6d2aa79000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libgssapi_krb5.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340\266\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=305456, ...}) = 0
mmap(NULL, 2401088, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d2a615000
mprotect(0x7f6d2a65d000, 2093056, PROT_NONE) = 0
mmap(0x7f6d2a85c000, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x47000) = 0x7f6d2a85c000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\260\34\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=2030544, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f6d2b724000
mmap(NULL, 4131552, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d2a224000
mprotect(0x7f6d2a40b000, 2097152, PROT_NONE) = 0
mmap(0x7f6d2a60b000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1e7000) = 0x7f6d2a60b000
mmap(0x7f6d2a611000, 15072, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f6d2a611000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpcre.so.3", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0 \25\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=464824, ...}) = 0
mmap(NULL, 2560264, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d29fb2000
mprotect(0x7f6d2a022000, 2097152, PROT_NONE) = 0
mmap(0x7f6d2a222000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x70000) = 0x7f6d2a222000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libkrb5.so.3", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\320\27\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=877056, ...}) = 0
mmap(NULL, 2972896, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d29cdc000
mprotect(0x7f6d29da2000, 2097152, PROT_NONE) = 0
mmap(0x7f6d29fa2000, 65536, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xc6000) = 0x7f6d29fa2000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libk5crypto.so.3", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0PC\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=199104, ...}) = 0
mmap(NULL, 2297976, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d29aaa000
mprotect(0x7f6d29ad8000, 2097152, PROT_NONE) = 0
mmap(0x7f6d29cd8000, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2e000) = 0x7f6d29cd8000
mmap(0x7f6d29cdb000, 120, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f6d29cdb000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libcom_err.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\200\23\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=14248, ...}) = 0
mmap(NULL, 2109608, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d298a6000
mprotect(0x7f6d298a9000, 2093056, PROT_NONE) = 0
mmap(0x7f6d29aa8000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0x7f6d29aa8000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libkrb5support.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0@'\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=43616, ...}) = 0
mmap(NULL, 2139080, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d2969b000
mprotect(0x7f6d296a5000, 2093056, PROT_NONE) = 0
mmap(0x7f6d298a4000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x9000) = 0x7f6d298a4000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0000b\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=144976, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f6d2b722000
mmap(NULL, 2221184, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d2947c000
mprotect(0x7f6d29496000, 2093056, PROT_NONE) = 0
mmap(0x7f6d29695000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19000) = 0x7f6d29695000
mmap(0x7f6d29697000, 13440, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f6d29697000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libkeyutils.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\320\22\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=14256, ...}) = 0
mmap(NULL, 2109456, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d29278000
mprotect(0x7f6d2927b000, 2093056, PROT_NONE) = 0
mmap(0x7f6d2947a000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0x7f6d2947a000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f6d2b720000
arch_prctl(ARCH_SET_FS, 0x7f6d2b720f80) = 0
mprotect(0x7f6d2a60b000, 16384, PROT_READ) = 0
mprotect(0x7f6d2947a000, 4096, PROT_READ) = 0
mprotect(0x7f6d29695000, 4096, PROT_READ) = 0
mprotect(0x7f6d2ae9a000, 4096, PROT_READ) = 0
mprotect(0x7f6d298a4000, 4096, PROT_READ) = 0
mprotect(0x7f6d29aa8000, 4096, PROT_READ) = 0
mprotect(0x7f6d29cd8000, 8192, PROT_READ) = 0
mprotect(0x7f6d2aa77000, 4096, PROT_READ) = 0
mprotect(0x7f6d29fa2000, 57344, PROT_READ) = 0
mprotect(0x7f6d2a222000, 4096, PROT_READ) = 0
mprotect(0x7f6d2a85c000, 8192, PROT_READ) = 0
mprotect(0x7f6d2ac96000, 4096, PROT_READ) = 0
mprotect(0x7f6d2b2b5000, 114688, PROT_READ) = 0
mprotect(0x7f6d2b504000, 4096, PROT_READ) = 0
mprotect(0x55dc368c5000, 12288, PROT_READ) = 0
mprotect(0x7f6d2b72f000, 4096, PROT_READ) = 0
munmap(0x7f6d2b728000, 27376)           = 0
set_tid_address(0x7f6d2b721250)         = 17676
set_robust_list(0x7f6d2b721260, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f6d29481cb0, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f6d2948e8a0}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f6d29481d50, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f6d2948e8a0}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
statfs("/sys/fs/selinux", 0x7fff5597fa90) = -1 ENOENT (No such file or directory)
statfs("/selinux", 0x7fff5597fa90)      = -1 ENOENT (No such file or directory)
brk(NULL)                               = 0x55dc37df1000
brk(0x55dc37e12000)                     = 0x55dc37e12000
openat(AT_FDCWD, "/proc/filesystems", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0444, st_size=0, ...}) = 0
read(3, "nodev\tsysfs\nnodev\trootfs\nnodev\tr"..., 1024) = 407
read(3, "", 1024)                       = 0
close(3)                                = 0
access("/etc/selinux/config", F_OK)     = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/dev/null", O_RDWR)   = 3
close(3)                                = 0
getpid()                                = 17676
openat(AT_FDCWD, "/proc/17676/fd", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
fstat(3, {st_mode=S_IFDIR|0500, st_size=0, ...}) = 0
getdents(3, /* 6 entries */, 32768)     = 144
getdents(3, /* 0 entries */, 32768)     = 0
close(3)                                = 0
getuid()                                = 0
geteuid()                               = 0
setresuid(-1, 0, -1)                    = 0
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(3)                                = 0
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(3)                                = 0
openat(AT_FDCWD, "/etc/nsswitch.conf", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=513, ...}) = 0
read(3, "# /etc/nsswitch.conf\n#\n# Example"..., 4096) = 513
read(3, "", 4096)                       = 0
close(3)                                = 0
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=27376, ...}) = 0
mmap(NULL, 27376, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f6d2b728000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_compat.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\240\22\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=39744, ...}) = 0
mmap(NULL, 2136256, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d2906e000
mprotect(0x7f6d29076000, 2097152, PROT_NONE) = 0
mmap(0x7f6d29276000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x8000) = 0x7f6d29276000
close(3)                                = 0
mprotect(0x7f6d29276000, 4096, PROT_READ) = 0
munmap(0x7f6d2b728000, 27376)           = 0
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=27376, ...}) = 0
mmap(NULL, 27376, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f6d2b728000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_nis.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0p \0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=47576, ...}) = 0
mmap(NULL, 2143624, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d28e62000
mprotect(0x7f6d28e6d000, 2093056, PROT_NONE) = 0
mmap(0x7f6d2906c000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xa000) = 0x7f6d2906c000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnsl.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220@\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=97176, ...}) = 0
mmap(NULL, 2202200, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d28c48000
mprotect(0x7f6d28c5f000, 2093056, PROT_NONE) = 0
mmap(0x7f6d28e5e000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x16000) = 0x7f6d28e5e000
mmap(0x7f6d28e60000, 6744, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f6d28e60000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_files.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P#\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=47568, ...}) = 0
mmap(NULL, 2168632, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f6d28a36000
mprotect(0x7f6d28a41000, 2093056, PROT_NONE) = 0
mmap(0x7f6d28c40000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xa000) = 0x7f6d28c40000
mmap(0x7f6d28c42000, 22328, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f6d28c42000
close(3)                                = 0
mprotect(0x7f6d28c40000, 4096, PROT_READ) = 0
mprotect(0x7f6d28e5e000, 4096, PROT_READ) = 0
mprotect(0x7f6d2906c000, 4096, PROT_READ) = 0
munmap(0x7f6d2b728000, 27376)           = 0
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 3
lseek(3, 0, SEEK_CUR)                   = 0
fstat(3, {st_mode=S_IFREG|0644, st_size=1565, ...}) = 0
mmap(NULL, 1565, PROT_READ, MAP_SHARED, 3, 0) = 0x7f6d2b72e000
lseek(3, 1565, SEEK_SET)                = 1565
munmap(0x7f6d2b72e000, 1565)            = 0
close(3)                                = 0
umask(022)                              = 022
openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=1683056, ...}) = 0
mmap(NULL, 1683056, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f6d2b585000
close(3)                                = 0
openat(AT_FDCWD, "/usr/share/locale/locale.alias", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=2995, ...}) = 0
read(3, "# Locale name alias data base.\n#"..., 4096) = 2995
read(3, "", 4096)                       = 0
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_CTYPE", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=199772, ...}) = 0
mmap(NULL, 199772, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f6d2b554000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache", O_RDONLY) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=26376, ...}) = 0
mmap(NULL, 26376, PROT_READ, MAP_SHARED, 3, 0) = 0x7f6d2b728000
close(3)                                = 0
futex(0x7f6d2a610a08, FUTEX_WAKE_PRIVATE, 2147483647) = 0
openat(AT_FDCWD, "/usr/lib/ssl/openssl.cnf", O_RDONLY) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=10998, ...}) = 0
read(3, "#\n# OpenSSL example configuratio"..., 4096) = 4096
read(3, "tableString, T61String (no BMPSt"..., 4096) = 4096
read(3, "eting an end user certificate as"..., 4096) = 2806
read(3, "", 4096)                       = 0
close(3)                                = 0
brk(0x55dc37e34000)                     = 0x55dc37e34000
openat(AT_FDCWD, "/root/.ssh/config", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ssh/ssh_config", O_RDONLY) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=1580, ...}) = 0
read(3, "\n# This is the ssh client system"..., 4096) = 1580
read(3, "", 4096)                       = 0
close(3)                                = 0
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(3)                                = 0
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(3)                                = 0
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=27376, ...}) = 0
mmap(NULL, 27376, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f6d2b54d000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/tls/haswell/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/tls/haswell/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/tls/haswell/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/tls/haswell/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/tls/haswell/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/tls/haswell/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/tls/haswell/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/tls/haswell", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/tls/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/tls/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/tls/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/tls/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/tls/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/tls/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/tls/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/tls", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/haswell/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/haswell/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/haswell/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/haswell/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/haswell/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/haswell/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/haswell/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/haswell", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64-linux-gnu", {st_mode=S_IFDIR|0755, st_size=12288, ...}) = 0
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/tls/haswell/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/tls/haswell/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/tls/haswell/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/tls/haswell/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/tls/haswell/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/tls/haswell/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/tls/haswell/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/tls/haswell", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/tls/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/tls/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/tls/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/tls/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/tls/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/tls/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/tls/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/tls", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/haswell/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/haswell/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/haswell/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/haswell/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/haswell/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/haswell/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/haswell/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/haswell", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64-linux-gnu", {st_mode=S_IFDIR|0755, st_size=20480, ...}) = 0
openat(AT_FDCWD, "/lib/tls/haswell/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/tls/haswell/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/tls/haswell/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/tls/haswell/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/tls/haswell/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/tls/haswell/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/tls/haswell/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/tls/haswell", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/tls/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/tls/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/tls/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/tls/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/tls/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/tls/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/tls/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/tls", 0x7fff5597c070)        = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/haswell/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/haswell/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/haswell/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/haswell/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/haswell/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/haswell/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/haswell/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/haswell", 0x7fff5597c070)    = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/avx512_1", 0x7fff5597c070)   = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib/x86_64", 0x7fff5597c070)     = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/lib", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
openat(AT_FDCWD, "/usr/lib/tls/haswell/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/tls/haswell/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/tls/haswell/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/tls/haswell/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/tls/haswell/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/tls/haswell/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/tls/haswell/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/tls/haswell", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/tls/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/tls/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/tls/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/tls/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/tls/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/tls/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/tls/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/tls", 0x7fff5597c070)    = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/haswell/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/haswell/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/haswell/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/haswell/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/haswell/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/haswell/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/haswell/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/haswell", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/avx512_1/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/avx512_1/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/avx512_1/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/avx512_1", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib/x86_64", 0x7fff5597c070) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/libnss_db.so.2", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/usr/lib", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
munmap(0x7f6d2b54d000, 27376)           = 0
openat(AT_FDCWD, "/etc/services", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=19183, ...}) = 0
read(3, "# Network services, Internet sty"..., 4096) = 4096
close(3)                                = 0
ioctl(0, TCGETS, {B38400 opost isig icanon echo ...}) = 0
getpid()                                = 17676
openat(AT_FDCWD, "/dev/urandom", O_RDONLY|O_NOCTTY|O_NONBLOCK) = 3
fstat(3, {st_mode=S_IFCHR|0666, st_rdev=makedev(1, 9), ...}) = 0
poll([{fd=3, events=POLLIN}], 1, 10)    = 1 ([{fd=3, revents=POLLIN}])
read(3, "E\234\256=\336E\355\205\347\267\241\232\250\366\5\1\252\370-{\242\3077uu%{\320\216\27\27*", 32) = 32
close(3)                                = 0
getuid()                                = 0
uname({sysname="Linux", nodename="ubuntu-s-1vcpu-1gb-sfo2-01", ...}) = 0
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(3)                                = 0
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(3)                                = 0
stat("/etc/resolv.conf", {st_mode=S_IFREG|0644, st_size=715, ...}) = 0
openat(AT_FDCWD, "/etc/host.conf", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=92, ...}) = 0
read(3, "# The \"order\" line is only used "..., 4096) = 92
read(3, "", 4096)                       = 0
close(3)                                = 0
futex(0x7f6d2a613ba4, FUTEX_WAKE_PRIVATE, 2147483647) = 0
openat(AT_FDCWD, "/etc/resolv.conf", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=715, ...}) = 0
read(3, "# This file is managed by man:sy"..., 4096) = 715
read(3, "", 4096)                       = 0
close(3)                                = 0
uname({sysname="Linux", nodename="ubuntu-s-1vcpu-1gb-sfo2-01", ...}) = 0
openat(AT_FDCWD, "/etc/hosts", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=635, ...}) = 0
read(3, "# Your system has configured 'ma"..., 4096) = 635
read(3, "", 4096)                       = 0
close(3)                                = 0
socket(AF_INET, SOCK_STREAM, IPPROTO_TCP) = 3
fcntl(3, F_SETFD, FD_CLOEXEC)           = 0
connect(3, {sa_family=AF_INET, sin_port=htons(22), sin_addr=inet_addr("127.0.0.1")}, 16) = 0
setsockopt(3, SOL_SOCKET, SO_KEEPALIVE, [1], 4) = 0
getpeername(3, {sa_family=AF_INET, sin_port=htons(22), sin_addr=inet_addr("127.0.0.1")}, [128->16]) = 0
getpeername(3, {sa_family=AF_INET, sin_port=htons(22), sin_addr=inet_addr("127.0.0.1")}, [128->16]) = 0
getsockname(3, {sa_family=AF_INET, sin_port=htons(34928), sin_addr=inet_addr("127.0.0.1")}, [128->16]) = 0
getsockname(3, {sa_family=AF_INET, sin_port=htons(34928), sin_addr=inet_addr("127.0.0.1")}, [128->16]) = 0
setresuid(-1, 0, -1)                    = 0
getuid()                                = 0
getgid()                                = 0
setresgid(0, 0, 0)                      = 0
setresuid(0, 0, 0)                      = 0
getgid()                                = 0
getegid()                               = 0
getuid()                                = 0
geteuid()                               = 0
stat("/root/.ssh", {st_mode=S_IFDIR|0700, st_size=4096, ...}) = 0
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=1565, ...}) = 0
mmap(NULL, 1565, PROT_READ, MAP_SHARED, 4, 0) = 0x7f6d2b553000
lseek(4, 1565, SEEK_SET)                = 1565
munmap(0x7f6d2b553000, 1565)            = 0
close(4)                                = 0
uname({sysname="Linux", nodename="ubuntu-s-1vcpu-1gb-sfo2-01", ...}) = 0
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=1565, ...}) = 0
mmap(NULL, 1565, PROT_READ, MAP_SHARED, 4, 0) = 0x7f6d2b553000
lseek(4, 1565, SEEK_SET)                = 1565
munmap(0x7f6d2b553000, 1565)            = 0
close(4)                                = 0
openat(AT_FDCWD, "/root/.ssh/id_rsa", O_RDONLY) = 4
fstat(4, {st_mode=S_IFREG|0600, st_size=1679, ...}) = 0
read(4, "-----BEGIN RSA PRIVATE KEY-----\n"..., 4096) = 1679
close(4)                                = 0
openat(AT_FDCWD, "/root/.ssh/id_rsa.pub", O_RDONLY) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=413, ...}) = 0
read(4, "ssh-rsa AAAAB3NzaC1yc2EAAAADAQAB"..., 4096) = 413
close(4)                                = 0
openat(AT_FDCWD, "/root/.ssh/id_rsa-cert", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.ssh/id_rsa-cert.pub", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=1565, ...}) = 0
mmap(NULL, 1565, PROT_READ, MAP_SHARED, 4, 0) = 0x7f6d2b553000
lseek(4, 1565, SEEK_SET)                = 1565
munmap(0x7f6d2b553000, 1565)            = 0
close(4)                                = 0
openat(AT_FDCWD, "/root/.ssh/id_dsa", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.ssh/id_dsa.pub", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.ssh/id_dsa-cert", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.ssh/id_dsa-cert.pub", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=1565, ...}) = 0
mmap(NULL, 1565, PROT_READ, MAP_SHARED, 4, 0) = 0x7f6d2b553000
lseek(4, 1565, SEEK_SET)                = 1565
munmap(0x7f6d2b553000, 1565)            = 0
close(4)                                = 0
openat(AT_FDCWD, "/root/.ssh/id_ecdsa", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.ssh/id_ecdsa.pub", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.ssh/id_ecdsa-cert", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.ssh/id_ecdsa-cert.pub", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=1565, ...}) = 0
mmap(NULL, 1565, PROT_READ, MAP_SHARED, 4, 0) = 0x7f6d2b553000
lseek(4, 1565, SEEK_SET)                = 1565
munmap(0x7f6d2b553000, 1565)            = 0
close(4)                                = 0
openat(AT_FDCWD, "/root/.ssh/id_ed25519", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.ssh/id_ed25519.pub", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.ssh/id_ed25519-cert", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.ssh/id_ed25519-cert.pub", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=1565, ...}) = 0
mmap(NULL, 1565, PROT_READ, MAP_SHARED, 4, 0) = 0x7f6d2b553000
lseek(4, 1565, SEEK_SET)                = 1565
munmap(0x7f6d2b553000, 1565)            = 0
close(4)                                = 0
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=1565, ...}) = 0
mmap(NULL, 1565, PROT_READ, MAP_SHARED, 4, 0) = 0x7f6d2b553000
lseek(4, 1565, SEEK_SET)                = 1565
munmap(0x7f6d2b553000, 1565)            = 0
close(4)                                = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGCHLD, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGCHLD, {sa_handler=0x55dc36625590, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
write(3, "SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ub"..., 41) = 41
read(3, "S", 1)                         = 1
read(3, "S", 1)                         = 1
read(3, "H", 1)                         = 1
read(3, "-", 1)                         = 1
read(3, "2", 1)                         = 1
read(3, ".", 1)                         = 1
read(3, "0", 1)                         = 1
read(3, "-", 1)                         = 1
read(3, "O", 1)                         = 1
read(3, "p", 1)                         = 1
read(3, "e", 1)                         = 1
read(3, "n", 1)                         = 1
read(3, "S", 1)                         = 1
read(3, "S", 1)                         = 1
read(3, "H", 1)                         = 1
read(3, "_", 1)                         = 1
read(3, "7", 1)                         = 1
read(3, ".", 1)                         = 1
read(3, "6", 1)                         = 1
read(3, "p", 1)                         = 1
read(3, "1", 1)                         = 1
read(3, " ", 1)                         = 1
read(3, "U", 1)                         = 1
read(3, "b", 1)                         = 1
read(3, "u", 1)                         = 1
read(3, "n", 1)                         = 1
read(3, "t", 1)                         = 1
read(3, "u", 1)                         = 1
read(3, "-", 1)                         = 1
read(3, "4", 1)                         = 1
read(3, "u", 1)                         = 1
read(3, "b", 1)                         = 1
read(3, "u", 1)                         = 1
read(3, "n", 1)                         = 1
read(3, "t", 1)                         = 1
read(3, "u", 1)                         = 1
read(3, "0", 1)                         = 1
read(3, ".", 1)                         = 1
read(3, "3", 1)                         = 1
read(3, "\r", 1)                        = 1
read(3, "\n", 1)                        = 1
fcntl(3, F_GETFL)                       = 0x2 (flags O_RDWR)
fcntl(3, F_SETFL, O_RDWR|O_NONBLOCK)    = 0
openat(AT_FDCWD, "/root/.ssh/known_hosts", O_RDONLY) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=2654, ...}) = 0
read(4, "|1|x5h/0tPR6ihuxjyKLumn4fNzR0k=|"..., 4096) = 2654
read(4, "", 4096)                       = 0
close(4)                                = 0
openat(AT_FDCWD, "/root/.ssh/known_hosts2", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ssh/ssh_known_hosts", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ssh/ssh_known_hosts2", O_RDONLY) = -1 ENOENT (No such file or directory)
getpid()                                = 17676
getpid()                                = 17676
write(3, "\0\0\5L\5\24\231\4\34\313\273\317\324\370\267\200\305\344\36\356!x\0\0\0010curve2"..., 1360) = 1360
select(4, [3], NULL, NULL, NULL)        = 1 (in [3])
read(3, "\0\0\0044\6\24-\264\336\260\213\22\376%#S\327\224I\242\267\217\0\0\1\2curve2"..., 8192) = 1080
getpid()                                = 17676
write(3, "\0\0\0,\6\36\0\0\0 \v~\335\357\224\224\271s\272\222\207F\343\340S\20\\\235\324M\37%"..., 48) = 48
select(4, [3], NULL, NULL, NULL)        = 1 (in [3])
read(3, "\0\0\1\4\n\37\0\0\0h\0\0\0\23ecdsa-sha2-nistp25"..., 8192) = 452
openat(AT_FDCWD, "/root/.ssh/known_hosts", O_RDONLY) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=2654, ...}) = 0
read(4, "|1|x5h/0tPR6ihuxjyKLumn4fNzR0k=|"..., 4096) = 2654
read(4, "", 4096)                       = 0
close(4)                                = 0
openat(AT_FDCWD, "/root/.ssh/known_hosts2", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ssh/ssh_known_hosts", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ssh/ssh_known_hosts2", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/dev/tty", O_RDWR)    = 4
close(4)                                = 0
openat(AT_FDCWD, "/dev/tty", O_RDWR)    = 4
ioctl(4, TCGETS, {B38400 opost isig icanon echo ...}) = 0
ioctl(4, TCGETS, {B38400 opost isig icanon echo ...}) = 0
ioctl(4, SNDCTL_TMR_CONTINUE or TCSETSF, {B38400 opost isig icanon echo ...}) = 0
ioctl(4, TCGETS, {B38400 opost isig icanon echo ...}) = 0
rt_sigaction(SIGALRM, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGHUP, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGINT, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGQUIT, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTERM, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTSTP, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTTIN, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTTOU, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
write(4, "The authenticity of host 'localh"..., 203The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:uMeG72tXQLEC346gie2FQ6xlh1+QU/u5e6NRWZYtjTA.
Are you sure you want to continue connecting (yes/no)? ) = 203
read(4, yes
"y", 1)                         = 1
read(4, "e", 1)                         = 1
read(4, "s", 1)                         = 1
read(4, "\n", 1)                        = 1
rt_sigaction(SIGALRM, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGHUP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGTERM, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGTSTP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGTTIN, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGTTOU, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
close(4)                                = 0
openat(AT_FDCWD, "/root/.ssh/known_hosts", O_WRONLY|O_CREAT|O_APPEND, 0666) = 4
lseek(4, 0, SEEK_END)                   = 2654
getpid()                                = 17676
fstat(4, {st_mode=S_IFREG|0644, st_size=2654, ...}) = 0
write(4, "|1|Tm3FzeSSD9isxpLXD84uA1/4Lxw=|"..., 222) = 222
close(4)                                = 0
write(2, "Warning: Permanently added 'loca"..., 76Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
) = 76
clock_gettime(CLOCK_BOOTTIME, {tv_sec=28719285, tv_nsec=791452634}) = 0
write(3, "\0\0\0\f\n\25\0\0\0\0\0\0\0\0\0\0", 16) = 16
getpid()                                = 17676
write(3, "[I\232QVh+g\0d\26\220\372\315\264U\200+\0\350i>\vN-\376wA\217\230%\3"..., 44) = 44
select(4, [3], NULL, NULL, NULL)        = 1 (in [3])
read(3, "\224y\253\35n\270\220\360\n\310\302\300\230\v;.\372\357\265\211-\273\210\263\325\371J4D\241\231\374"..., 8192) = 44
getpid()                                = 17676
write(3, "#\324\223\274\317\373\367\276\"5\31\324X&aBy\3\317\227\236\3b\3 \324\214\305%tD\244"..., 60) = 60
select(4, [3], NULL, NULL, NULL)        = 1 (in [3])
read(3, "+\201\275S\372\256\243\317\224\201E\263h\254_\346\272\212\265\210P2\217\242\363\303y\234\236l\"\275"..., 8192) = 52
getpid()                                = 17676
write(3, "\10\37s\204\354\32\234A5\177\365\211\246\211`\25\207R\322kD\v\231\265n\326)\271\246\"yR"..., 372) = 372
select(4, [3], NULL, NULL, NULL)        = 1 (in [3])
read(3, "@-vf#<\366\250\271.~\310\266{\221t\1\17(\336I\2174\202h\2(\367\3\377yr"..., 8192) = 52
stat("/root/.ssh/id_dsa", 0x7fff5597c730) = -1 ENOENT (No such file or directory)
stat("/root/.ssh/id_ecdsa", 0x7fff5597c730) = -1 ENOENT (No such file or directory)
stat("/root/.ssh/id_ed25519", 0x7fff5597c730) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/dev/tty", O_RDWR)    = 4
close(4)                                = 0
openat(AT_FDCWD, "/dev/tty", O_RDWR)    = 4
ioctl(4, TCGETS, {B38400 opost isig icanon echo ...}) = 0
ioctl(4, TCGETS, {B38400 opost isig icanon echo ...}) = 0
ioctl(4, SNDCTL_TMR_CONTINUE or TCSETSF, {B38400 opost isig icanon -echo ...}) = 0
ioctl(4, TCGETS, {B38400 opost isig icanon -echo ...}) = 0
rt_sigaction(SIGALRM, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGHUP, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGINT, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGQUIT, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGTERM, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGTSTP, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGTTIN, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGTTOU, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
write(4, "root@localhost's password: ", 27root@localhost's password: ) = 27
read(4, "\n", 1)                        = 1
write(4, "\n", 1
)                       = 1
ioctl(4, TCGETS, {B38400 opost isig icanon -echo ...}) = 0
ioctl(4, SNDCTL_TMR_CONTINUE or TCSETSF, {B38400 opost isig icanon echo ...}) = 0
ioctl(4, TCGETS, {B38400 opost isig icanon echo ...}) = 0
rt_sigaction(SIGALRM, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGHUP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGTERM, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGTSTP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGTTIN, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
rt_sigaction(SIGTTOU, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, NULL, 8) = 0
close(4)                                = 0
getpid()                                = 17676
write(3, "\221\252\261\373\342S\26\233\266\353uO0\315\202a\234#\345\263\255E\312\223#(!\246\375\306\361\33"..., 84) = 84
select(4, [3], NULL, NULL, NULL)        = 1 (in [3])
read(3, "\334\35\n\255mI\272\204\240\n\220\27\322\5\307\337\342\350g\332zjn\361\5\277\357&h\256\177L"..., 8192) = 52
write(2, "Permission denied, please try ag"..., 38Permission denied, please try again.
) = 38
openat(AT_FDCWD, "/dev/tty", O_RDWR)    = 4
close(4)                                = 0
openat(AT_FDCWD, "/dev/tty", O_RDWR)    = 4
ioctl(4, TCGETS, {B38400 opost isig icanon echo ...}) = 0
ioctl(4, TCGETS, {B38400 opost isig icanon echo ...}) = 0
ioctl(4, SNDCTL_TMR_CONTINUE or TCSETSF, {B38400 opost isig icanon -echo ...}) = 0
ioctl(4, TCGETS, {B38400 opost isig icanon -echo ...}) = 0
rt_sigaction(SIGALRM, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGHUP, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGINT, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGQUIT, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGTERM, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGTSTP, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGTTIN, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
rt_sigaction(SIGTTOU, {sa_handler=0x55dc36682bb0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7f6d2a262fd0}, 8) = 0
write(4, "root@localhost's password: ", 27root@localhost's password: ) = 27
read(4, 0x7fff5597bcff, 1)              = ? ERESTARTSYS (To be restarted if SA_RESTART is set)
strace: Process 17676 detached
```

### What happens when you type google.com in your browser?

```
strace curl google.com
execve("/usr/bin/curl", ["curl", "google.com"], 0x7ffe1e682f58 /* 23 vars */) = 0
brk(NULL)                               = 0x561628a60000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=27376, ...}) = 0
mmap(NULL, 27376, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f5643039000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libcurl.so.4", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220\305\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=530888, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f5643037000
mmap(NULL, 2626952, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f5642b97000
mprotect(0x7f5642c15000, 2097152, PROT_NONE) = 0
mmap(0x7f5642e15000, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x7e000) = 0x7f5642e15000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libz.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220\37\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=116960, ...}) = 0
mmap(NULL, 2212016, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f564297a000
mprotect(0x7f5642996000, 2093056, PROT_NONE) = 0
mmap(0x7f5642b95000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b000) = 0x7f5642b95000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0000b\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=144976, ...}) = 0
mmap(NULL, 2221184, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f564275b000
mprotect(0x7f5642775000, 2093056, PROT_NONE) = 0
mmap(0x7f5642974000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19000) = 0x7f5642974000
mmap(0x7f5642976000, 13440, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f5642976000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\260\34\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=2030544, ...}) = 0
mmap(NULL, 4131552, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f564236a000
mprotect(0x7f5642551000, 2097152, PROT_NONE) = 0
mmap(0x7f5642751000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1e7000) = 0x7f5642751000
mmap(0x7f5642757000, 15072, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f5642757000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libnghttp2.so.14", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\200H\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=153352, ...}) = 0
mmap(NULL, 2248560, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f5642145000
mprotect(0x7f5642168000, 2093056, PROT_NONE) = 0
mmap(0x7f5642367000, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x22000) = 0x7f5642367000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libidn2.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\360\24\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=116656, ...}) = 0
mmap(NULL, 2211864, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f5641f28000
mprotect(0x7f5641f44000, 2093056, PROT_NONE) = 0
mmap(0x7f5642143000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b000) = 0x7f5642143000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/librtmp.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\200J\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=113584, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f5643035000
mmap(NULL, 2208688, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f5641d0c000
mprotect(0x7f5641d27000, 2093056, PROT_NONE) = 0
mmap(0x7f5641f26000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1a000) = 0x7f5641f26000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libpsl.so.5", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\22\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=55136, ...}) = 0
mmap(NULL, 2150504, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f5641afe000
mprotect(0x7f5641b0b000, 2093056, PROT_NONE) = 0
mmap(0x7f5641d0a000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xc000) = 0x7f5641d0a000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libssl.so.1.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220\325\1\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=577312, ...}) = 0
mmap(NULL, 2673024, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f5641871000
mprotect(0x7f56418f2000, 2093056, PROT_NONE) = 0
mmap(0x7f5641af1000, 53248, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x80000) = 0x7f5641af1000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libcrypto.so.1.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\200\7\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=2917216, ...}) = 0
mmap(NULL, 5025632, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f56413a6000
mprotect(0x7f5641641000, 2093056, PROT_NONE) = 0
mmap(0x7f5641840000, 188416, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x29a000) = 0x7f5641840000
mmap(0x7f564186e000, 12128, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f564186e000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libgssapi_krb5.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340\266\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=305456, ...}) = 0
mmap(NULL, 2401088, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f564115b000
mprotect(0x7f56411a3000, 2093056, PROT_NONE) = 0
mmap(0x7f56413a2000, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x47000) = 0x7f56413a2000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libldap_r-2.4.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\20\321\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=327024, ...}) = 0
mmap(NULL, 2431720, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f5640f09000
mprotect(0x7f5640f57000, 2093056, PROT_NONE) = 0
mmap(0x7f5641156000, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x4d000) = 0x7f5641156000
mmap(0x7f5641159000, 6888, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f5641159000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/liblber-2.4.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0`*\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=55544, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f5643033000
mmap(NULL, 2150888, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f5640cfb000
mprotect(0x7f5640d08000, 2093056, PROT_NONE) = 0
mmap(0x7f5640f07000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xc000) = 0x7f5640f07000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libunistring.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\200\365\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=1562664, ...}) = 0
mmap(NULL, 3660040, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f564097d000
mprotect(0x7f5640af7000, 2097152, PROT_NONE) = 0
mmap(0x7f5640cf7000, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x17a000) = 0x7f5640cf7000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libgnutls.so.30", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0@\210\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=1461856, ...}) = 0
mmap(NULL, 3562312, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f5640617000
mprotect(0x7f564076f000, 2097152, PROT_NONE) = 0
mmap(0x7f564096f000, 53248, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x158000) = 0x7f564096f000
mmap(0x7f564097c000, 2888, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f564097c000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libhogweed.so.4", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220o\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=219976, ...}) = 0
mmap(NULL, 2315032, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f56403e1000
mprotect(0x7f5640415000, 2097152, PROT_NONE) = 0
mmap(0x7f5640615000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x34000) = 0x7f5640615000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libnettle.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\20\201\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=219304, ...}) = 0
mmap(NULL, 2314384, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f56401ab000
mprotect(0x7f56401df000, 2093056, PROT_NONE) = 0
mmap(0x7f56403de000, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x33000) = 0x7f56403de000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libgmp.so.10", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\200\223\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=526688, ...}) = 0
mmap(NULL, 2621888, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563ff2a000
mprotect(0x7f563ffa9000, 2097152, PROT_NONE) = 0
mmap(0x7f56401a9000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x7f000) = 0x7f56401a9000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f5643031000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libdl.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\16\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=14560, ...}) = 0
mmap(NULL, 2109712, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563fd26000
mprotect(0x7f563fd29000, 2093056, PROT_NONE) = 0
mmap(0x7f563ff28000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0x7f563ff28000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libkrb5.so.3", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\320\27\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=877056, ...}) = 0
mmap(NULL, 2972896, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563fa50000
mprotect(0x7f563fb16000, 2097152, PROT_NONE) = 0
mmap(0x7f563fd16000, 65536, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xc6000) = 0x7f563fd16000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libk5crypto.so.3", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0PC\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=199104, ...}) = 0
mmap(NULL, 2297976, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563f81e000
mprotect(0x7f563f84c000, 2097152, PROT_NONE) = 0
mmap(0x7f563fa4c000, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2e000) = 0x7f563fa4c000
mmap(0x7f563fa4f000, 120, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f563fa4f000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libcom_err.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\200\23\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=14248, ...}) = 0
mmap(NULL, 2109608, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563f61a000
mprotect(0x7f563f61d000, 2093056, PROT_NONE) = 0
mmap(0x7f563f81c000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0x7f563f81c000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libkrb5support.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0@'\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=43616, ...}) = 0
mmap(NULL, 2139080, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563f40f000
mprotect(0x7f563f419000, 2093056, PROT_NONE) = 0
mmap(0x7f563f618000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x9000) = 0x7f563f618000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libresolv.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\00008\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=101168, ...}) = 0
mmap(NULL, 2206336, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563f1f4000
mprotect(0x7f563f20b000, 2097152, PROT_NONE) = 0
mmap(0x7f563f40b000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x17000) = 0x7f563f40b000
mmap(0x7f563f40d000, 6784, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f563f40d000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f564302f000
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libsasl2.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340*\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=109296, ...}) = 0
mmap(NULL, 2204648, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563efd9000
mprotect(0x7f563eff2000, 2097152, PROT_NONE) = 0
mmap(0x7f563f1f2000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19000) = 0x7f563f1f2000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libgssapi.so.3", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0 \333\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=265712, ...}) = 0
mmap(NULL, 2361240, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563ed98000
mprotect(0x7f563edd5000, 2097152, PROT_NONE) = 0
mmap(0x7f563efd5000, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3d000) = 0x7f563efd5000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libp11-kit.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\263\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=1237640, ...}) = 0
mmap(NULL, 3334512, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563ea69000
mprotect(0x7f563eb83000, 2097152, PROT_NONE) = 0
mmap(0x7f563ed83000, 81920, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x11a000) = 0x7f563ed83000
mmap(0x7f563ed97000, 368, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f563ed97000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libtasn1.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220)\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=75776, ...}) = 0
mmap(NULL, 2171592, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563e856000
mprotect(0x7f563e867000, 2097152, PROT_NONE) = 0
mmap(0x7f563ea67000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x11000) = 0x7f563ea67000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libkeyutils.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\320\22\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=14256, ...}) = 0
mmap(NULL, 2109456, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563e652000
mprotect(0x7f563e655000, 2093056, PROT_NONE) = 0
mmap(0x7f563e854000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0x7f563e854000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libheimntlm.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0*\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=35360, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f564302d000
mmap(NULL, 2130512, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563e449000
mprotect(0x7f563e451000, 2093056, PROT_NONE) = 0
mmap(0x7f563e650000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x7000) = 0x7f563e650000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libkrb5.so.26", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\260\315\1\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=573464, ...}) = 0
mmap(NULL, 2671504, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563e1bc000
mprotect(0x7f563e243000, 2093056, PROT_NONE) = 0
mmap(0x7f563e442000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x86000) = 0x7f563e442000
mmap(0x7f563e448000, 912, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f563e448000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libasn1.so.8", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220\233\1\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=661696, ...}) = 0
mmap(NULL, 2756848, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563df1a000
mprotect(0x7f563dfb8000, 2097152, PROT_NONE) = 0
mmap(0x7f563e1b8000, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x9e000) = 0x7f563e1b8000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libhcrypto.so.4", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0`s\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=217560, ...}) = 0
mmap(NULL, 2316904, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563dce4000
mprotect(0x7f563dd17000, 2093056, PROT_NONE) = 0
mmap(0x7f563df16000, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x32000) = 0x7f563df16000
mmap(0x7f563df19000, 2664, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f563df19000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libroken.so.18", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\300N\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=88680, ...}) = 0
mmap(NULL, 2184240, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563dace000
mprotect(0x7f563dae3000, 2093056, PROT_NONE) = 0
mmap(0x7f563dce2000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x14000) = 0x7f563dce2000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libffi.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0@\27\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=31032, ...}) = 0
mmap(NULL, 2127368, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563d8c6000
mprotect(0x7f563d8cd000, 2093056, PROT_NONE) = 0
mmap(0x7f563dacc000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x6000) = 0x7f563dacc000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libwind.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0000\16\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=165880, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f564302b000
mmap(NULL, 2261040, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563d69d000
mprotect(0x7f563d6c5000, 2093056, PROT_NONE) = 0
mmap(0x7f563d8c4000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x27000) = 0x7f563d8c4000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libheimbase.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340(\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=60400, ...}) = 0
mmap(NULL, 2156760, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563d48e000
mprotect(0x7f563d49c000, 2093056, PROT_NONE) = 0
mmap(0x7f563d69b000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xd000) = 0x7f563d69b000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libhx509.so.5", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\20\1\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=300888, ...}) = 0
mmap(NULL, 2397224, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563d244000
mprotect(0x7f563d28a000, 2093056, PROT_NONE) = 0
mmap(0x7f563d489000, 16384, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x45000) = 0x7f563d489000
mmap(0x7f563d48d000, 1064, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f563d48d000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/libsqlite3.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\300\314\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=1082648, ...}) = 0
mmap(NULL, 3179736, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563cf3b000
mprotect(0x7f563d03f000, 2093056, PROT_NONE) = 0
mmap(0x7f563d23e000, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x103000) = 0x7f563d23e000
mmap(0x7f563d243000, 1240, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f563d243000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libcrypt.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\r\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=39208, ...}) = 0
mmap(NULL, 2322976, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563cd03000
mprotect(0x7f563cd0c000, 2093056, PROT_NONE) = 0
mmap(0x7f563cf0b000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x8000) = 0x7f563cf0b000
mmap(0x7f563cf0d000, 184864, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f563cf0d000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\200\272\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=1700792, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f5643029000
mmap(NULL, 3789144, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f563c965000
mprotect(0x7f563cb02000, 2093056, PROT_NONE) = 0
mmap(0x7f563cd01000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19c000) = 0x7f563cd01000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f5643027000
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f5643024000
arch_prctl(ARCH_SET_FS, 0x7f5643024740) = 0
mprotect(0x7f5642751000, 16384, PROT_READ) = 0
mprotect(0x7f563cd01000, 4096, PROT_READ) = 0
mprotect(0x7f563cf0b000, 4096, PROT_READ) = 0
mprotect(0x7f5642974000, 4096, PROT_READ) = 0
mprotect(0x7f563ff28000, 4096, PROT_READ) = 0
mprotect(0x7f563d23e000, 12288, PROT_READ) = 0
mprotect(0x7f563f81c000, 4096, PROT_READ) = 0
mprotect(0x7f563f40b000, 4096, PROT_READ) = 0
mprotect(0x7f563dce2000, 4096, PROT_READ) = 0
mprotect(0x7f563e1b8000, 4096, PROT_READ) = 0
mprotect(0x7f563d69b000, 4096, PROT_READ) = 0
mprotect(0x7f563df16000, 8192, PROT_READ) = 0
mprotect(0x7f563d8c4000, 4096, PROT_READ) = 0
mprotect(0x7f563d489000, 12288, PROT_READ) = 0
mprotect(0x7f563dacc000, 4096, PROT_READ) = 0
mprotect(0x7f563e442000, 16384, PROT_READ) = 0
mprotect(0x7f563e650000, 4096, PROT_READ) = 0
mprotect(0x7f563e854000, 4096, PROT_READ) = 0
mprotect(0x7f563ea67000, 4096, PROT_READ) = 0
mprotect(0x7f563ed83000, 40960, PROT_READ) = 0
mprotect(0x7f563efd5000, 8192, PROT_READ) = 0
mprotect(0x7f563f1f2000, 4096, PROT_READ) = 0
mprotect(0x7f563f618000, 4096, PROT_READ) = 0
mprotect(0x7f563fa4c000, 8192, PROT_READ) = 0
mprotect(0x7f563fd16000, 57344, PROT_READ) = 0
mprotect(0x7f56401a9000, 4096, PROT_READ) = 0
mprotect(0x7f56403de000, 8192, PROT_READ) = 0
mprotect(0x7f5640615000, 4096, PROT_READ) = 0
mprotect(0x7f5642b95000, 4096, PROT_READ) = 0
mprotect(0x7f5640cf7000, 12288, PROT_READ) = 0
mprotect(0x7f5642143000, 4096, PROT_READ) = 0
mprotect(0x7f564096f000, 49152, PROT_READ) = 0
mprotect(0x7f5640f07000, 4096, PROT_READ) = 0
mprotect(0x7f5641156000, 8192, PROT_READ) = 0
mprotect(0x7f56413a2000, 8192, PROT_READ) = 0
mprotect(0x7f5641840000, 180224, PROT_READ) = 0
mprotect(0x7f5641af1000, 36864, PROT_READ) = 0
mprotect(0x7f5641d0a000, 4096, PROT_READ) = 0
mprotect(0x7f5641f26000, 4096, PROT_READ) = 0
mprotect(0x7f5642367000, 4096, PROT_READ) = 0
mprotect(0x7f5642e15000, 12288, PROT_READ) = 0
mprotect(0x56162866e000, 16384, PROT_READ) = 0
mprotect(0x7f5643040000, 4096, PROT_READ) = 0
munmap(0x7f5643039000, 27376)           = 0
set_tid_address(0x7f5643024a10)         = 17691
set_robust_list(0x7f5643024a20, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f5642760cb0, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f5642760d50, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
futex(0x7f563ed970e0, FUTEX_WAKE_PRIVATE, 2147483647) = 0
brk(NULL)                               = 0x561628a60000
brk(0x561628a81000)                     = 0x561628a81000
getrandom("\x3e", 1, GRND_NONBLOCK)     = 1
stat("/etc/gnutls/default-priorities", 0x7fff58559c80) = -1 ENOENT (No such file or directory)
pipe([3, 4])                            = 0
close(3)                                = 0
close(4)                                = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f56423a8fd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
futex(0x7f564186f810, FUTEX_WAKE_PRIVATE, 2147483647) = 0
futex(0x7f564186f804, FUTEX_WAKE_PRIVATE, 2147483647) = 0
futex(0x7f564186dc7c, FUTEX_WAKE_PRIVATE, 2147483647) = 0
futex(0x7f564186f6c4, FUTEX_WAKE_PRIVATE, 2147483647) = 0
futex(0x7f564186f65c, FUTEX_WAKE_PRIVATE, 2147483647) = 0
futex(0x7f564186f650, FUTEX_WAKE_PRIVATE, 2147483647) = 0
brk(0x561628aa2000)                     = 0x561628aa2000
futex(0x7f564186f7fc, FUTEX_WAKE_PRIVATE, 2147483647) = 0
futex(0x7f564186f7b8, FUTEX_WAKE_PRIVATE, 2147483647) = 0
futex(0x7f564186f7b0, FUTEX_WAKE_PRIVATE, 2147483647) = 0
openat(AT_FDCWD, "/usr/lib/ssl/openssl.cnf", O_RDONLY) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=10998, ...}) = 0
read(3, "#\n# OpenSSL example configuratio"..., 4096) = 4096
read(3, "tableString, T61String (no BMPSt"..., 4096) = 4096
read(3, "eting an end user certificate as"..., 4096) = 2806
read(3, "", 4096)                       = 0
close(3)                                = 0
socket(AF_INET6, SOCK_DGRAM, IPPROTO_IP) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=1683056, ...}) = 0
mmap(NULL, 1683056, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f5642e89000
close(3)                                = 0
openat(AT_FDCWD, "/usr/share/locale/locale.alias", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=2995, ...}) = 0
read(3, "# Locale name alias data base.\n#"..., 4096) = 2995
read(3, "", 4096)                       = 0
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_IDENTIFICATION", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=252, ...}) = 0
mmap(NULL, 252, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f564303f000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache", O_RDONLY) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=26376, ...}) = 0
mmap(NULL, 26376, PROT_READ, MAP_SHARED, 3, 0) = 0x7f5642e82000
close(3)                                = 0
futex(0x7f5642756a08, FUTEX_WAKE_PRIVATE, 2147483647) = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_MEASUREMENT", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=23, ...}) = 0
mmap(NULL, 23, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f564303e000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_TELEPHONE", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=47, ...}) = 0
mmap(NULL, 47, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f564303d000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_ADDRESS", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=131, ...}) = 0
mmap(NULL, 131, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f564303c000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_NAME", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=62, ...}) = 0
mmap(NULL, 62, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f564303b000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_PAPER", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=34, ...}) = 0
mmap(NULL, 34, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f564303a000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_MESSAGES", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_MESSAGES/SYS_LC_MESSAGES", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=48, ...}) = 0
mmap(NULL, 48, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f5643039000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_MONETARY", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=270, ...}) = 0
mmap(NULL, 270, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f5642e81000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_COLLATE", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=1516558, ...}) = 0
mmap(NULL, 1516558, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f563c7f2000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_TIME", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=3360, ...}) = 0
mmap(NULL, 3360, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f5642e80000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_NUMERIC", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=50, ...}) = 0
mmap(NULL, 50, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f5642e7f000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_CTYPE", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=199772, ...}) = 0
mmap(NULL, 199772, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f5642e4e000
close(3)                                = 0
openat(AT_FDCWD, "/root/.curlrc", O_RDONLY) = -1 ENOENT (No such file or directory)
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
brk(0x561628adb000)                     = 0x561628adb000
ioctl(0, TIOCGWINSZ, {ws_row=33, ws_col=160, ws_xpixel=2240, ws_ypixel=1254}) = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f56423a8fd0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f563bff1000
mprotect(0x7f563bff2000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f563c7f0fb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f563c7f19d0, tls=0x7f563c7f1700, child_tidptr=0x7f563c7f19d0) = 17692
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
poll(NULL, 0, 4)                        = 0 (Timeout)
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
socket(AF_INET, SOCK_STREAM, IPPROTO_TCP) = 3
setsockopt(3, SOL_TCP, TCP_NODELAY, [1], 4) = 0
setsockopt(3, SOL_SOCKET, SO_KEEPALIVE, [1], 4) = 0
setsockopt(3, SOL_TCP, TCP_KEEPIDLE, [60], 4) = 0
setsockopt(3, SOL_TCP, TCP_KEEPINTVL, [60], 4) = 0
fcntl(3, F_GETFL)                       = 0x2 (flags O_RDWR)
fcntl(3, F_SETFL, O_RDWR|O_NONBLOCK)    = 0
connect(3, {sa_family=AF_INET, sin_port=htons(80), sin_addr=inet_addr("216.58.194.206")}, 16) = -1 EINPROGRESS (Operation now in progress)
poll([{fd=3, events=POLLOUT|POLLWRNORM}], 1, 0) = 0 (Timeout)
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
poll([{fd=3, events=POLLOUT}], 1, 199)  = 1 ([{fd=3, revents=POLLOUT}])
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
poll([{fd=3, events=POLLOUT|POLLWRNORM}], 1, 0) = 1 ([{fd=3, revents=POLLOUT|POLLWRNORM}])
getsockopt(3, SOL_SOCKET, SO_ERROR, [0], [4]) = 0
getpeername(3, {sa_family=AF_INET, sin_port=htons(80), sin_addr=inet_addr("216.58.194.206")}, [128->16]) = 0
getsockname(3, {sa_family=AF_INET, sin_port=htons(33418), sin_addr=inet_addr("64.225.117.10")}, [128->16]) = 0
sendto(3, "GET / HTTP/1.1\r\nHost: google.com"..., 74, MSG_NOSIGNAL, NULL, 0) = 74
poll([{fd=3, events=POLLIN|POLLPRI|POLLRDNORM|POLLRDBAND}], 1, 0) = 0 (Timeout)
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
poll([{fd=3, events=POLLIN}], 1, 197)   = 1 ([{fd=3, revents=POLLIN}])
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
poll([{fd=3, events=POLLIN|POLLPRI|POLLRDNORM|POLLRDBAND}], 1, 0) = 1 ([{fd=3, revents=POLLIN|POLLRDNORM}])
recvfrom(3, "HTTP/1.1 301 Moved Permanently\r\n"..., 102400, 0, NULL, NULL) = 528
fstat(1, {st_mode=S_IFCHR|0600, st_rdev=makedev(136, 0), ...}) = 0
write(1, "<HTML><HEAD><meta http-equiv=\"co"..., 79<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
) = 79
write(1, "<TITLE>301 Moved</TITLE></HEAD><"..., 38<TITLE>301 Moved</TITLE></HEAD><BODY>
) = 38
write(1, "<H1>301 Moved</H1>\n", 19<H1>301 Moved</H1>
)    = 19
write(1, "The document has moved\n", 23The document has moved
) = 23
write(1, "<A HREF=\"http://www.google.com/\""..., 44<A HREF="http://www.google.com/">here</A>.
) = 44
write(1, "</BODY></HTML>\r\n", 16</BODY></HTML>
)      = 16
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
close(3)                                = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
brk(0x561628ac1000)                     = 0x561628ac1000
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f564276d8a0}, NULL, 8) = 0
futex(0x7f564186f948, FUTEX_WAKE_PRIVATE, 2147483647) = 0
fstat(0, {st_mode=S_IFCHR|0600, st_rdev=makedev(136, 0), ...}) = 0
fstat(0, {st_mode=S_IFCHR|0600, st_rdev=makedev(136, 0), ...}) = 0
fstat(0, {st_mode=S_IFCHR|0600, st_rdev=makedev(136, 0), ...}) = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```
