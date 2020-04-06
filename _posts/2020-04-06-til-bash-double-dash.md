---
layout: post
title: '#TIL: Double dash "--" option in bash'
date: 2020-04-06
---

It signals the end of command options, after which only positional parameters are accepted.

**Example use**: lets say you want to grep a file for the string `-v` - normally `-v` will be considered 
the option to reverse the matching meaning (only show lines that do not match), but with `--` you can grep
for string -v like this

```
grep -- -v file
```
