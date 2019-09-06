# Loop over output of another command and run ssh

We need a new file descriptor here

```
while read -u 3 -r ip; do
  ssh "$ip" date
done 3< <(get data from somewhere)
```
