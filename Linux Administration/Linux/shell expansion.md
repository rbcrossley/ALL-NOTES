![](_resources/Pasted%20image%2020240314110242.png)
# Process of bash command
- Command is expanded.
- Word splitting is applied.
- Quotes are being removed.
- Command is being executed.
This also applies to the program name that we want to execute.
# Pitfalls
![](_resources/Pasted%20image%2020240314164019.png)
```
ls ./*
```
is safe in case of bad ambiguous file names
```
./file.txt
```
Refer to filenames like this.
