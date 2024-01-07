# `&`
runs program in background.
```cmd
./sayHello 1000>hellos & ./sayHi 1000>his & ./printDate 1000 > dates &
```
Once in background to get out of it, use 
`kill %1 %2 %3`

# The relation of OR and AND in || and && operators
`&& operator`
`0(?)=0`
If 1 input is false, overall output is false. That means don't execute the rest of ANDed commands.
`|| operator`
`1(?)=1`
If 1 input is true, overall output is true. That means don't execute the rest of ORed commands.