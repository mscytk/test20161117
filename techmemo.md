https://qiita.com/t_ymgt/items/7f13c89b08b889da2146
cybereason-sensorプロセスですが、やっていることは、ひたすら /procの内容をチェックしている感じかもしれません。
これじゃあcbramといいrootkit検出できないじゃないかと。
```shell
$ sudo strace -e trace=file -p $(pgrep -f cybereason-sensor) -f

[pid  4644] openat(AT_FDCWD, "/proc/1772/fd", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 45
[pid  4644] readlink("/proc/1772/fd/0", "/dev/null", 1023) = 9
[pid  4644] readlink("/proc/1772/fd/1", "socket:[24039]", 1023) = 14
[pid  4644] readlink("/proc/1772/fd/2", "/var/log/httpd/error_log", 1023) = 24
[pid  4644] readlink("/proc/1772/fd/3", "socket:[25129]", 1023) = 14
[pid  4644] readlink("/proc/1772/fd/4", "socket:[25133]", 1023) = 14
[pid  4644] readlink("/proc/1772/fd/5", "pipe:[25143]", 1023) = 12
[pid  4644] readlink("/proc/1772/fd/6", "pipe:[25143]", 1023) = 12
[pid  4644] readlink("/proc/1772/fd/7", "/var/log/httpd/ssl_error_log", 1023) = 28
[pid  4644] readlink("/proc/1772/fd/8", "/service/data/httpd/logs/error.l"..., 1023) = 34
[pid  4644] readlink("/proc/1772/fd/9", "/var/log/httpd/access_log", 1023) = 25
[pid  4644] readlink("/proc/1772/fd/10", "/var/log/httpd/ssl_access_log", 1023) = 29
[pid  4644] readlink("/proc/1772/fd/11", "/var/log/httpd/ssl_request_log", 1023) = 30
[pid  4644] readlink("/proc/1772/fd/12", "/service/data/httpd/logs/access."..., 1023) = 35
[pid  4644] readlink("/proc/1772/fd/13", "/service/data/httpd/logs/ssl_acc"..., 1023) = 39
[pid  4644] readlink("/proc/1772/fd/14", "/var/lib/sss/mc/initgroups", 1023) = 26
[pid  4644] readlink("/proc/1772/fd/15", "socket:[26086]", 1023) = 14
[pid  4644] readlink("/proc/1772/fd/16", "anon_inode:[eventpoll]", 1023) = 22
[pid  4644] openat(AT_FDCWD, "/proc/1773/fd", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 45
```
