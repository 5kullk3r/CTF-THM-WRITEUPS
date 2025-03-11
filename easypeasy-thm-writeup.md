EASY PEASY — THM CTF — WRITEUP
==============================

[![5kullk3r](https://miro.medium.com/v2/resize:fill:88:88/1*dmbNkD5D-u45r44go_cf0g.png)](https://medium.com/?source=post_page---byline--926f023a1eb5---------------------------------------)

[5kullk3r](https://medium.com/?source=post_page---byline--926f023a1eb5---------------------------------------)


8 min read


Hello everyone! This is an easy room from the TryHackMe platform called **“Easy Peasy”**. The room covers basic tool usage such as **nmap**, **feroxbuster**, and **gobuster.**

While the room is marked as “easy,” there are a few twists in the process that might make some steps challenging. But that’s where this write-up comes in to guide you through!

My goal is to help you understand each step and provide clear explanations so that anyone, whether a beginner or experienced, can follow along and understand the reasoning behind each action. I hope this write-up makes the process smoother and easier to grasp.

Enough talk — let’s dive right in, and I hope you enjoy the journey! :)

![No need to have that expression on your face, I know you’re just here for the sauce😹](https://miro.medium.com/v2/resize:fit:720/format:webp/0*zQ-FJ1T2xT2QWSvf)

First, we deploy the machine. While it’s booting up, go ahead and open your terminal tab and get it ready

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*Hs2crqMEezqsDPa4)

Once it’s up, let’s paste the IP address and connect to the network. This is what we have in front of us:

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*u2-bH3Ung4263Jt4)

I know there’s nothing here for now, so the next step is to find the open ports. Let’s use `nmap` to uncover them

First, I run the following command to scan specific ports on the target machine:

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*yCNbgHYkjqMRJ9Li)

nmap -sV -p- 10.10.14.65 — open

`-sV`: Enables version detection to identify the versions of services running on open ports.

`-p-`: Scans all 65535 ports on the target machine.

`10.10.14.65`: The target IP address you are scanning.

`--open`: Displays only the open ports, filtering out closed ones from the results

While this command runs and gives us the output, I’m also running the following command to see if I can find anything else or uncover more details

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*T6O6PwN9MPRVNTRq)

`nmap -sCV -p90,6498,65524 10.10.14.65`

*   `-sCV`: Runs the default scripts, performs service and version detection, and conducts basic scanning.
*   `-p90,6498,65524`: This specifies the ports we want to scan (90, 6498, and 65524).
*   `10.10.14.65`: The target IP address

![While copying and pasting this command, make sure to change the IP to your target’s IP — unless you want a bizarre response in the terminal](https://miro.medium.com/v2/resize:fit:400/format:webp/0*3HZ3E2hPp81i4-Pf)

By running this command, we immediately spot the answers to the three questions:

*   3 open ports
*   nginx 1.16.1
*   Apache

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*91C1MMvs4lNVCuUA)

TASK 2:
=======

Next, in our terminal, we’ll use **feroxbuster** (you could use **gobuster** if you’d like, but I’m more used to running feroxbuster).

Run the following command:

_feroxbuster -u_ [_http://10.10.14.65/_](http://10.10.14.65/) _-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt_

![captionless image](https://miro.medium.com/v2/resize:fit:1294/format:webp/0*LIh7SO-ILgtrM5lB)

Once it’s done, we get the following results, which prompt me to open them. I then copy and paste the results into two different tabs for further exploration.

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*aEvKDnKs9KaB65MU)![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*uHEMedC19-LFz1YK)

Upon initially seeing it, I thought it might be steganography. I tried downloading the images and running them through a tool, but there were no results. Then, I decided to inspect the page.

**In hindsight, starting by inspecting the page would have been a better approach**.

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*yK6pYHP6yeGuQ2kg)

Here, we don’t find anything, so we move on to inspect the next page.

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*okhLIE_zn1Qe7pS3)

Then, we come across this, and from prior experience and by looking at it closely, I can tell it’s likely encoded in Base64.

So, I copy it and head over to [https://www.base64decode.org/](https://www.base64decode.org/) to decode it. This leads me to the first flag.

![flag{f1rs7_fl4g}](https://miro.medium.com/v2/resize:fit:1190/format:webp/0*bAyiOePPolmJgo-M)

Now, to figure out the other flags, I refer to the three port numbers mentioned in the Nmap scan and navigate to port 65524, which is open with HTTP. This leads me to the Apache page

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*zTu9H1flaiFdqojx)

Sitting on the Apache page, my first instinct is to check the `robots.txt` file for any web crawlers. And sure enough, it leads me to this

![User-Agent:a18672860d0510e5ab6699730763b250](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*6Muhz70hJhPBuxs-)

I notice this is an MD5 hash, so I use [https://md5hashing.net/](https://md5hashing.net/) to decode it and uncover the second flag.

![flag{1m_s3c0nd_fl4g}](https://miro.medium.com/v2/resize:fit:580/format:webp/0*67i6mgRgKzuFOOGv)

While inspecting the Apache page, I noticed something hidden in plain sight. In the page’s HTML source code, I found the third flag:

Fl4g 3 : flag{9fdafbd64c47471a8f54cd3fc64cd312}

![Summary of the 3 flags](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*0vW9mt6KI3y38h4YlNCOmQ.png)

Now, this is where things get a bit twisted. After looking around for a bit, I finally realized something when I saw this while inspecting the same page, just above the third flag, next to the word ‘hidden’

![ObsJmP173N2X6dOrAgEAL0Vu](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*Kntiorx4tki9XBlK)

Now, with this clue, we need to figure out what to do next. I use [https://www.dcode.fr/cipher-identifier](https://www.dcode.fr/cipher-identifier) and identify that it’s encoded in Base62.

After decoding the string `ObsJmP173N2X6dOrAgEAL0Vu`, I get `/n0th1ng3ls3m4tt3r`, which looks like a directory format

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*w1NHeqacXQKntQCutGBkVw.png)

I then use the decoded directory path, which leads me to this page with an image.

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*cHgVj84EDfH4l8VQ)

So, I proceed to download both the image and the wordlist. Then, in the terminal, I perform the steganography decryption process by running the following command first:

-> **_stegcracker bbi.jpg easype.txt_**

So, I proceed to download both the image (`bbi.jpg`) and the wordlist (`easype.txt`). Then, in the terminal, I perform the steganography decryption process by running the following command, which leads me to:

![password: mypasswordforthatjob](https://miro.medium.com/v2/resize:fit:1188/format:webp/0*TjQ2_EEBDHRHTgrp)

I perform the following command to extract the hidden content:

-> **_steghide — extract -sf bbi.jpg_**

*   `--extract`: This flag tells `steghide` to extract hidden data from the image.
*   `-sf bbi.jpg`: The `-sf` flag specifies the input file, in this case, `bbi.jpg`

![captionless image](https://miro.medium.com/v2/resize:fit:670/format:webp/0*iNISBFTZ4zBbOXDf)

Upon discovering the username `boring`, it prompts the thought of SSH/FTP to me. Since we know the data is in binary, we decode it online using [this site](https://cryptii.com/pipes/binary-decoder).

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*yNp_GOtNe30TwA2I)![Password: iconvertedmypasswordtobinary](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*2W6c7_F3mJElt9Bd)

After decoding, we get the password: `iconvertedmypasswordtobinary`.

With this, I head to the terminal and attempt to access the SSH service using the following command: ssh boring@10.10.14.65 -p 6498

![port 6498 as in the nmap it was the ssh port](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*HIgIWne8vta7ujUp)

I enter the password and gain access to the system.
Wasting no time, I immediately use the `ls` command to list the files present.
Among the files, I find `user.txt`.
I then use the `cat` command to open it and retrieve the contents, which reveals this:

![captionless image](https://miro.medium.com/v2/resize:fit:1118/format:webp/0*MkEcB97OLdtM7_TB)

This immediately shows that ROT decoding is needed. I head over to dCode ROT and enter the ROT-encoded flag string.

Voila! I decode it and get the flag for `user.txt`

![flag{n0wits33msn0rm4l}](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*fTln_7ZuXD6qWZRF)

Now that I have the `user.txt` flag, I turn my attention to gaining access to `root.txt`. To do that, I start by checking for any potential ways to elevate my privilege

First, I run `sudo -l`, but it doesn’t yield any helpful results. So, I move on to check the crontab process for any interesting entries: **_cat /etc/crontab_**

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*E5BGNlSqNLv9BUIi)

But, here is the peculiar entry:

* * * * * root cd /var/www/ && sudo bash .mysecretcronjob.sh

This line indicates that there is a cron job running every minute as the `root` user that executes the script `.mysecretcronjob.sh` inside `/var/www/`.

The job is using `sudo`, which may give us an opportunity for privilege escalation.

I refer to the **Pentest Monkey Cheat Sheet** for a reverse shell payload. I then execute the payload in the bash file, and simultaneously, I open a new terminal tab to start the listener: {**_nc -lvnp 4444_**}

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*EI6mPohHc0JvezPW)

By executing the reverse shell payload successfully, I gain access to the system. I then proceed with the following 4 commands:

*   `ls` — To list the contents.
*   `cd ~` — To navigate to the home directory.
*   `ls -la` — To list all files, including hidden ones.
*   `cat .root.txt` — To read the root flag and complete the objective.

This leads us to the root flag: `flag{63a9f0ea7bb98050796b649e85481845}`, and with that, we have successfully completed the room!

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*pTP2K7hRJW9MMteU)

CONCLUSION :
============

I hope this writeup walkthrough from my end was helpful to you guys. I thought it would be a quick room, but it challenged me, and in the end, it was fun to complete.

Now that I’ve gotten through it, I hope it helps you and gets you through the room as well. This being my first writeup, I plan on putting out more like these in the future!

If you guys want me to cover any specific room or challenge, or if you have any queries, feel free to drop a comment.

I’ll check it out and get back to you as soon as I can. Also, you can find all of my writeups and future ones on my Medium: https://medium.com/@5kullk3r/easy-peasy-thm-ctf-writeup-926f023a1eb5.

Imma bounce for now, but I’ll catch you all in the next writeup!