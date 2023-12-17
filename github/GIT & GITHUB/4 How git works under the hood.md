![](_resources/Pasted%20image%2020231112195845.png)
![](_resources/Pasted%20image%2020231112195902.png)
![](_resources/Pasted%20image%2020231112195921.png)

- `git init` initializes git repository. It creates a new empty git repository. Git repo will be initially empty even if directory where you initialize git isn't empty and contains other files and directories.
- Default branch is called master.
- git has its own file system. Git stores objects. All objects are stored in this folder.
## Git object types
- blob
- commit
- tree
- annotated tag
blob stores files(text,videos,images etc).
tree stores directories information.
commit stores various versions of our project.
annotated tag is a persistent text pointer to a specific commit.

```
echo "Hello,Git" | git hash-object --stdin -w
```
Hash=folder name+ file name
- Structure of git db is similar to json.
- Two key-value pairs with same key not possible in json and git.
- In json, it's possible to have same values in different key value pairs, in git it's not possible because in git, key is generated based on value.

Same hash function with same hash input will always produce same output.

# Types of hash functions
- MD5(128 bit)
- SHA1(160 bit=40 hexadecimal characters)
- SHA256
- SHA384
- SHA512

# SHA1
- Even small change of input data will lead to creation of completely different hash.
- `shasum` command to calculate the hash.
- `echo -n "Hello, Git"` means no new line after the end of text.
# How many files could git store in the same repository?
$2^{160}$
# What is the chance of producing same hash for different files?
$1/2^{160} * 1/2^{160}$



Reference:
https://stashchuk.com/static/git/4-how-git-works-under-the-hood.pdf

