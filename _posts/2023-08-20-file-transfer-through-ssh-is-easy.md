---
title: File transfer through SSH is easy!
date: 2023-08-20 11:48:05 -0300
categories: [blog, tutorial]
tags: [ssh, linux ]
image:
  path: "/assets/img/posts/ethernet-cables-plugged-in-network-switch.jpg"
  alt: "Ethernet Cables Plugged in Network Switch"
---

For newcomers, SSH itself might look hard at first and file transferring even more complicated. How to connect to a remote machine to send or copy files from it?

I'm going to show you that it is not as complicated as it looks. As a matter of fact, it's quite easy!

First let's define the appearance of our remote connection for our examples:

- User: `dev`
- Address: `123.45.67.89`

> Replace it with your real remote machine connection while following the examples
{: .prompt-tip }

## The interactive way

Let's use the **SSH File Transfer Protocol**, or SFTP.

With this tool, we connect to our remote machine and use it with a _bash-like_ shell. Let's see how it works:

```bash
sftp dev@123.45.67.89
```

With SFTP we rely on file browsing to either receive or send files, after opening our connection let's explore the remote and local working directories. 

Inside the remote we can navigate freely using `cd` and `ls` commands. See all available commands with `help`.

```bash
sftp> pwd
Remote working directory: /home/dev

sftp> lpwd
Local working directory: /c/Users/User
```

Sending files from 💻 local machine to the 🌐 remote machine:

```bash
sftp> put # Press Tab
myfile1.txt 
myfile2.txt

sftp> put myfile2.txt # 👈 Send myfile2.txt to the remote working directory
```

Receiving files or folders from 🌐 remote machine to 💻 local machine:

```bash
sftp> ls
file1.txt
file2.txt
folder

sftp> get file1.txt # 👈 Gets file1.txt to the local working directory

sftp> get -r folder
        # 👆 To get an entire directory
```

## The non-interactive way

We will be using the **Secure Copy Protocol**, or SCP.

It's a nice and quick way and all it takes is a _ssh-like_ command with the file paths, a login prompt and there you go! 

Sending files from 💻 local machine to the 🌐 remote machine:

```bash
scp ./file.txt dev@123.45.67.89:/home/dev/file.txt
    # 👆 Your local file path   👆 Where to place it on the remote
```

Receiving files or folders from 🌐 remote machine to 💻 local machine:

```bash
                                      # 👇 Where to place it on the local
scp dev@123.45.67.89:/home/dev/file.txt ./file.txt
                  # 👆 The remote file path

scp -r dev@123.45.67.89:/home/dev/folder ./folder
  # 👆 To copy an entire directory
```

> Although this is an easy way for a quick file transfer there are some caveats.
{: .prompt-warning }

> According to [OpenSSH](https://en.wikipedia.org/wiki/OpenSSH "OpenSSH") developers in April 2019, SCP is outdated, inflexible and not readily fixed; they recommend the use of more modern protocols like [SFTP](https://en.wikipedia.org/wiki/Secure_file_transfer_program "Secure file transfer program") and [rsync](https://en.wikipedia.org/wiki/Rsync "Rsync") for file transfer.[[3]](https://en.wikipedia.org/wiki/Secure_copy_protocol#cite_note-3) As of OpenSSH version 9.0, `scp` client therefore uses SFTP for file transfers by default instead of the legacy SCP/RCP protocol.[[4](https://en.wikipedia.org/wiki/Secure_copy_protocol#cite_note-ossh9-4)
> 
> &mdash; <cite>Wikipedia</cite>

## Conclusion

As you can see, file transferring through SSH is not that hard. There are a few methods to accomplish it and after you get used to it, it's just like doing a regular connection. 

Now you can get that file or send it to your remote box, have fun!

## Attributions and Sources

- Photo by Brett Sayles from Pexels: https://www.pexels.com/photo/ethernet-cables-plugged-in-network-switch-2881224/
- https://en.wikipedia.org/wiki/Secure_copy_protocol
- https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol
- https://superuser.com/questions/134901/whats-the-difference-between-scp-and-sftp
- https://www.digitalocean.com/community/tutorials/how-to-use-sftp-to-securely-transfer-files-with-a-remote-server
