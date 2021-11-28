---
tags: [bash]
---
# Loop over output of another command and run ssh

We need a new file descriptor here

```
while read -u 3 -r ip; do
  ssh "$ip" date
done 3< <(get data from somewhere)
```

# Find lines that exist in two files

This is technically not a bash thingy. It is a way to only find lines of two files which exist in both. Basically an intersections of two sets.

```
> cat a
a
b
c
d
e
f

> cat b
d
f
g
e
c

> grep -f a b
d
f
e
c
```

The python equivalent is

```
>>> a = set(["a","b"])
>>> b = set(["b"])
>>> a & b
{'b'}
```

# echo to stderr

```
>&2 echo "error"
```
