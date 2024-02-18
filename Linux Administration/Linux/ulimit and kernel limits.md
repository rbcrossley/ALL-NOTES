`nproc` is the number of processes limit.
# Solution to increase file descriptor limits
```
*    soft    nofile 8192
*    hard    nofile 8192
```
at the end of `/etc/security/limits.conf`
# How to load test
References:
https://serverfault.com/questions/48717/practical-maximum-open-file-descriptors-ulimit-n-for-a-high-volume-system?rq=1