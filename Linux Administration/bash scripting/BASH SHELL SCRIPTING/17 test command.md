```
test expression
or
[ expression ]
```

```mermaid
flowchart TD
    A[Evaluate Expression] --> B{Value of Expression}
    B -->|Yes| C[0 exit status]
    B -->|No| D[1 exit status]
```

- In the second form of the command, the [ ] (brackets) must be surrounded by blank spaces.
- You must test explicitly for file names in the C shell. File-name substitution (globbing) causes the shell script to exit.
```mermaid
flowchart TD
    A[-d FILE] --> B{File is a Directory?}
    B -->|Yes| C[TRUE]
    B -->|No| D[FALSE]
```
```mermaid
flowchart TD
    A[-e FILE] --> B{File exists?}
    B -->|Yes| C[TRUE]
    B -->|No| D[FALSE]
```

```mermaid
flowchart TD
    A[-a FILE] --> B{File exists?}
    B -->|Yes| C[TRUE]
    B -->|No| D[FALSE]
```

```mermaid
flowchart TD
    A[-a FILE] --> B{File exists?}
    B -->|Yes| C[TRUE]
    B -->|No| D[FALSE]
```
```mermaid
flowchart TD
    A[-N FILE] --> B{File modified since last read?}
    B -->|Yes| C[TRUE]
    B -->|No| D[FALSE]
```
For more information:
https://www.ibm.com/docs/en/aix/7.2?topic=t-test-command

![8277b5dddfa0530c37ff593b5f4d474e.png](../_resources/8277b5dddfa0530c37ff593b5f4d474e.png)