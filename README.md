# drivers_PZ_1

## Создание программы на языке C
Команды для создания программы
```Terminal
nano devzero.c
gcc -o devzero devzero.c
```
Программа для чтения /dev/zero
```C
#include <fcntl.h>
#include <stdio.h>
#include <unistd.h>

int main() {
  char buf[100];
  int fd = open("/dev/zero", O_RDONLY);
  read(fd, buf, 100);
  close(fd);

  return 0;
}
```

## Результат работы strace
При запуске команды `strace ./devzero` получаем такой результат
```
execve("./devzero", ["./devzero"], 0x7fffe6d08e20 /* 22 vars */) = 0
brk(NULL)                               = 0x5cefae611000
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x723848a13000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=23595, ...}) = 0
mmap(NULL, 23595, PROT_READ, MAP_PRIVATE, 3, 0) = 0x723848a0d000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220\243\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
fstat(3, {st_mode=S_IFREG|0755, st_size=2125328, ...}) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
mmap(NULL, 2170256, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x723848600000
mmap(0x723848628000, 1605632, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x28000) = 0x723848628000
mmap(0x7238487b0000, 323584, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b0000) = 0x7238487b0000
mmap(0x7238487ff000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1fe000) = 0x7238487ff000
mmap(0x723848805000, 52624, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x723848805000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x723848a0a000
arch_prctl(ARCH_SET_FS, 0x723848a0a740) = 0
set_tid_address(0x723848a0aa10)         = 2992
set_robust_list(0x723848a0aa20, 24)     = 0
rseq(0x723848a0b060, 0x20, 0, 0x53053053) = 0
mprotect(0x7238487ff000, 16384, PROT_READ) = 0
mprotect(0x5cef87d5b000, 4096, PROT_READ) = 0
mprotect(0x723848a4b000, 8192, PROT_READ) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
munmap(0x723848a0d000, 23595)           = 0
openat(AT_FDCWD, "/dev/zero", O_RDONLY) = 3
read(3, "\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0"..., 100) = 100
close(3)                                = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```
