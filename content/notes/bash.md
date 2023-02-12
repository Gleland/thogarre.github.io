---
title: Bash
summary: "Bash notes"
draft: false
hideReadMore: true
---

test file for notes functionality.

Simple for loop:
```bash
for x in a b c; do echo $x;done
```

the same loop with xargs:
```bash
# back up of existing .tmp files
ls -a | grep .tmp | xargs -I@ cp @ @.bak
```


Commands can be sent via ssh:
```bash
ssh $host -- date # for quick one-liners
```
[Heredocs](https://en.wikipedia.org/wiki/Here_document) are useful to send more involved commands over ssh:

```bash
# trivial commands for sake of example
ssh $host << 'EOF'
    for i in $(seq 1 10); do
        mkdir -p /tmp/tmp_$1
    done
    # check to see they all exist
    ls tmp_*
EOF
```

moving a file or groups of files using bash's expansion expression (works with ZSH as well):

```bash
# move a csv file to a text file
mv intermediate_data{.csv,.txt}
```

more helpful when doing it via globbing:

```
# move all .txt files with bak to backup in the current directory
mv database_backup_*{.bak,.backup}
```
