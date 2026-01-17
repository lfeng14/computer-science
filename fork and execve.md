- 示例
```
franz@ubuntu:~/src$ cat demo.c 
#include <unistd.h>
#include <stdio.h>

int main() {
 char * const argv[] = {
  "/bin/bash","-c","env", NULL, 
 };
 char * const envp[] = {
  "HELLO=WORLD", NULL, 
 };
 execve("/bin/bash", argv, envp);
 print("Hello, World!\n");
}
```
- 构建
  ```
  gcc demo.c -o demo
  ```
- 用strace分析执行哪些系统调用
  ```
  strace ./demo
  execve("./demo", ["./demo"], 0xffffd7f1fcf0 /* 23 vars */) = 0
  brk(NULL)                               = 0xc7fe3e0e9000
  mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xe45ddc4e1000
  faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=34027, ...}) = 0
  mmap(NULL, 34027, PROT_READ, MAP_PRIVATE, 3, 0) = 0xe45ddc4d8000
  close(3)                                = 0
  openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
  read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\360\206\2\0\0\0\0\0"..., 832) = 832
  fstat(3, {st_mode=S_IFREG|0755, st_size=1722920, ...}) = 0
  mmap(NULL, 1892240, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_DENYWRITE, -1, 0) = 0xe45ddc2da000
  mmap(0xe45ddc2e0000, 1826704, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0) = 0xe45ddc2e0000
  munmap(0xe45ddc2da000, 24576)           = 0
  munmap(0xe45ddc49e000, 40848)           = 0
  mprotect(0xe45ddc479000, 81920, PROT_NONE) = 0
  mmap(0xe45ddc48d000, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19d000) = 0xe45ddc48d000
  mmap(0xe45ddc492000, 49040, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xe45ddc492000
  close(3)                                = 0
  set_tid_address(0xe45ddc4e1fb0)         = 405945
  set_robust_list(0xe45ddc4e1fc0, 24)     = 0
  rseq(0xe45ddc4e2600, 0x20, 0, 0xd428bc00) = 0
  mprotect(0xe45ddc48d000, 12288, PROT_READ) = 0
  mprotect(0xc7fe29e3f000, 4096, PROT_READ) = 0
  mprotect(0xe45ddc4e6000, 8192, PROT_READ) = 0
  prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
  munmap(0xe45ddc4d8000, 34027)           = 0
  execve("/bin/bash", ["/bin/bash", "-c", "env"], 0xffffc85d8118 /* 1 var */) = 0
  brk(NULL)                               = 0xb39daef54000
  mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xe71ed0ae6000
  faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=34027, ...}) = 0
  mmap(NULL, 34027, PROT_READ, MAP_PRIVATE, 3, 0) = 0xe71ed0add000
  close(3)                                = 0
  openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libtinfo.so.6", O_RDONLY|O_CLOEXEC) = 3
  read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\0\0\0\0\0\0\0\0"..., 832) = 832
  fstat(3, {st_mode=S_IFREG|0644, st_size=265416, ...}) = 0
  mmap(NULL, 329928, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_DENYWRITE, -1, 0) = 0xe71ed0a5c000
  mmap(0xe71ed0a60000, 264392, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0) = 0xe71ed0a60000
  munmap(0xe71ed0a5c000, 16384)           = 0
  munmap(0xe71ed0aa1000, 47304)           = 0
  mprotect(0xe71ed0a8f000, 53248, PROT_NONE) = 0
  mmap(0xe71ed0a9c000, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3c000) = 0xe71ed0a9c000
  close(3)                                = 0
  openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
  read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\360\206\2\0\0\0\0\0"..., 832) = 832
  fstat(3, {st_mode=S_IFREG|0755, st_size=1722920, ...}) = 0
  mmap(NULL, 1892240, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_DENYWRITE, -1, 0) = 0xe71ed0892000
  mmap(0xe71ed08a0000, 1826704, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0) = 0xe71ed08a0000
  munmap(0xe71ed0892000, 57344)           = 0
  munmap(0xe71ed0a5e000, 8080)            = 0
  mprotect(0xe71ed0a39000, 81920, PROT_NONE) = 0
  mmap(0xe71ed0a4d000, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19d000) = 0xe71ed0a4d000
  mmap(0xe71ed0a52000, 49040, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xe71ed0a52000
  close(3)                                = 0
  mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xe71ed0adb000
  set_tid_address(0xe71ed0adb0f0)         = 405945
  set_robust_list(0xe71ed0adb100, 24)     = 0
  rseq(0xe71ed0adb740, 0x20, 0, 0xd428bc00) = 0
  mprotect(0xe71ed0a4d000, 12288, PROT_READ) = 0
  mprotect(0xe71ed0a9c000, 16384, PROT_READ) = 0
  mprotect(0xb39d909bb000, 20480, PROT_READ) = 0
  mprotect(0xe71ed0aeb000, 8192, PROT_READ) = 0
  prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
  munmap(0xe71ed0add000, 34027)           = 0
  openat(AT_FDCWD, "/dev/tty", O_RDWR|O_NONBLOCK) = 3
  close(3)                                = 0
  getrandom("\xc3\x45\x16\x41\xd5\xf3\x38\xb9", 8, GRND_NONBLOCK) = 8
  brk(NULL)                               = 0xb39daef54000
  brk(0xb39daef75000)                     = 0xb39daef75000
  getuid()                                = 1000
  getgid()                                = 1000
  geteuid()                               = 1000
  getegid()                               = 1000
  rt_sigprocmask(SIG_BLOCK, NULL, [], 8)  = 0
  rt_sigaction(SIGCHLD, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGCHLD, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTART}, 8) = 0
  rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTSTP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTSTP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTTIN, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTTIN, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTTOU, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTTOU, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigprocmask(SIG_BLOCK, NULL, [], 8)  = 0
  rt_sigaction(SIGQUIT, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  uname({sysname="Linux", nodename="ubuntu", ...}) = 0
  getcwd("/home/franz/src", 4096)         = 16
  getpid()                                = 405945
  getppid()                               = 405942
  socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
  connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
  close(3)                                = 0
  socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
  connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
  close(3)                                = 0
  newfstatat(AT_FDCWD, "/etc/nsswitch.conf", {st_mode=S_IFREG|0644, st_size=526, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  openat(AT_FDCWD, "/etc/nsswitch.conf", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=526, ...}) = 0
  read(3, "# /etc/nsswitch.conf\n#\n# Example"..., 4096) = 526
  read(3, "", 4096)                       = 0
  fstat(3, {st_mode=S_IFREG|0644, st_size=526, ...}) = 0
  close(3)                                = 0
  openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=1757, ...}) = 0
  lseek(3, 0, SEEK_SET)                   = 0
  read(3, "root:x:0:0:root:/root:/bin/bash\n"..., 4096) = 1757
  close(3)                                = 0
  getpid()                                = 405945
  getppid()                               = 405942
  getpid()                                = 405945
  getppid()                               = 405942
  getpgid(0)                              = 405942
  ioctl(2, TIOCGPGRP, [405942])           = 0
  rt_sigaction(SIGCHLD, {sa_handler=0xb39d908a7d20, sa_mask=[], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTART}, 8) = 0
  prlimit64(0, RLIMIT_NPROC, NULL, {rlim_cur=79540, rlim_max=79540}) = 0
  rt_sigprocmask(SIG_BLOCK, NULL, [], 8)  = 0
  getpeername(0, 0xffffd5e56258, [16])    = -1 ENOTSOCK (Socket operation on non-socket)
  rt_sigprocmask(SIG_BLOCK, NULL, [], 8)  = 0
  newfstatat(AT_FDCWD, ".", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/local/sbin/env", 0xffffd5e55a78, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/local/bin/env", 0xffffd5e55a78, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/sbin/env", 0xffffd5e55a78, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/bin/env", {st_mode=S_IFREG|0755, st_size=68368, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/bin/env", {st_mode=S_IFREG|0755, st_size=68368, ...}, 0) = 0
  geteuid()                               = 1000
  getegid()                               = 1000
  getuid()                                = 1000
  getgid()                                = 1000
  faccessat(AT_FDCWD, "/usr/bin/env", X_OK) = 0
  newfstatat(AT_FDCWD, "/usr/bin/env", {st_mode=S_IFREG|0755, st_size=68368, ...}, 0) = 0
  geteuid()                               = 1000
  getegid()                               = 1000
  getuid()                                = 1000
  getgid()                                = 1000
  faccessat(AT_FDCWD, "/usr/bin/env", R_OK) = 0
  rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGCHLD, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTART}, {sa_handler=0xb39d908a7d20, sa_mask=[], sa_flags=SA_RESTART}, 8) = 0
  execve("/usr/bin/env", ["env"], 0xb39daef60640 /* 4 vars */) = 0
  brk(NULL)                               = 0xc369e15f8000
  mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xe57df918e000
  faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=34027, ...}) = 0
  mmap(NULL, 34027, PROT_READ, MAP_PRIVATE, 3, 0) = 0xe57df9185000
  close(3)                                = 0
  openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
  read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\360\206\2\0\0\0\0\0"..., 832) = 832
  fstat(3, {st_mode=S_IFREG|0755, st_size=1722920, ...}) = 0
  mmap(NULL, 1892240, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_DENYWRITE, -1, 0) = 0xe57df8f87000
  mmap(0xe57df8f90000, 1826704, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0) = 0xe57df8f90000
  munmap(0xe57df8f87000, 36864)           = 0
  munmap(0xe57df914e000, 28560)           = 0
  mprotect(0xe57df9129000, 81920, PROT_NONE) = 0
  mmap(0xe57df913d000, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19d000) = 0xe57df913d000
  mmap(0xe57df9142000, 49040, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xe57df9142000
  close(3)                                = 0
  set_tid_address(0xe57df918eff0)         = 405945
  set_robust_list(0xe57df918f000, 24)     = 0
  rseq(0xe57df918f640, 0x20, 0, 0xd428bc00) = 0
  mprotect(0xe57df913d000, 12288, PROT_READ) = 0
  mprotect(0xc369a9faf000, 4096, PROT_READ) = 0
  mprotect(0xe57df9193000, 8192, PROT_READ) = 0
  prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
  munmap(0xe57df9185000, 34027)           = 0
  getrandom("\x42\xf3\x04\xe6\x99\x36\xa1\xd0", 8, GRND_NONBLOCK) = 8
  brk(NULL)                               = 0xc369e15f8000
  brk(0xc369e1619000)                     = 0xc369e1619000
  fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0), ...}) = 0
  write(1, "PWD=/home/franz/src\n", 20PWD=/home/franz/src
  )   = 20
  write(1, "HELLO=WORLD\n", 12HELLO=WORLD
  )           = 12
  write(1, "SHLVL=0\n", 8SHLVL=0
  )                = 8
  write(1, "_=/usr/bin/env\n", 15_=/usr/bin/env
  )        = 15
  close(1)                                = 0
  close(2)                                = 0
  exit_group(0)                           = ?
  +++ exited with 0 +++
  ```
  - 说明linux系统内可以通过fork拷贝状态机也可以通过execve命令reset重置状态机
  
