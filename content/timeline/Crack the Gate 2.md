---
title: Crack the Gate 2 - PicoCTF
date: 2026-01-01
categories: CTF
tags:
  - ratelimiting
  - picoCTF
  - web
---
## Introduction

Hello, I've decided to grind picoCTF this year :)

Unlike my other writeups where I go into detail about my process, for picoCTF challenges, I plan to be straightforward (there might be exceptions) especially for the easy challenges.

## Solving the Challenge

> We’ve received a tip that the system might still trust user-controlled headers. Your objective is to bypass the rate-limiting restriction and log in using the known email address: **ctf-player@picoctf.org** and uncover the hidden secret.

After starting the challenge, we get the email "**ctf-player@picoctf.org**" and a txt file(**passwords.txt**) containing the possible password for the given email. 

Since the challenge description already tells us the goal, we can focus on bypassing the **rate-limiting restriction**.

Trying to manually enter different passwords results in a **429 Too Many Requests** response.

![Image Description](/images/Crack%20the%20Gate%202-0.png)

However, this restriction can be bypassed by using a common method: adding the `X-Forwarded-For` header with any value.

![Image Description](/images/Crack%20the%20Gate%202-1%201.png)

To solve the challenge, I setup a "**Pitchfork attack**" in Burpsuite Intruder.

- Payload 1: Use numbers as the value of `X-Forwarded-For` header to bypass the **rate-limiting restrictions**.
- Payload 2: Use the given passwords list.

![Image Description](/images/Crack%20the%20Gate%202-2.png)

![Image Description](/images/Crack%20the%20Gate%202-3.png)

Now, we just need to start the attack and check the response that has a different length to get the `FLAG`.

![Image Description](/images/Crack%20the%20Gate%202-4.png)

-***Datsuraku147***