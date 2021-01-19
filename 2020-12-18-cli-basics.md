# Command line basics

- [Command line basics](#command-line-basics)
  - [Agenda](#agenda)
  - [Resources](#resources)
  - [Session](#session)

## Agenda

1. Review basics of working with `ssh` in the command line
2. Use UI (like AWS, DO, CC cloud) to set up a VM (Ubuntu 18.04 or 20.04)
3. `ssh` into the VM
4. Navigating files & directories, history, sensitive info (prepend `"  "`, `grep`, `find`)
5. Working with files & directories (`pwd`,`cd`, `mkdir`, `rm`, `ls`, `vim`)
6. Other useful commands (`sudo`, `apt-get`, `reboot`, `exit`)
7. Combining stuff (`wc`, `echo`, `cat`, `>`, `>>`, `|`, `head`, `tail`, `sort`)
8. Loops
9. Scripts (arguments, interpolation)

## Resources

- [SW Carpentry shell novice lesson](https://swcarpentry.github.io/shell-novice/)
- [DO ssh essentials](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)
- [Windows WSL](https://docs.microsoft.com/en-us/windows/wsl/)

## Session

Sign up: https://www.digitalocean.com/,
$100 free credit for 60 days.

Log in with your email/password.

Walk through of the UI.

Create a new droplet:

- image
- CPU/RAM/SSD, basic, optimized etc
- region
- monitoring
- ssh key (see [this](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-2) tutorial)
- backup, additional space, managed DB

Once the droplet is running, go to the droplet page, explore the tabs:

- IP, floating IP
- Monitoring
- Resizing
- destroy

Open up the command line:

- options on Mac: [Xcode](https://developer.apple.com/xcode/), [brew](https://brew.sh/), [VS Code](https://code.visualstudio.com/), [iTerm2](https://iterm2.com/)
- Windows: [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10), [VS Code](https://code.visualstudio.com/)
- RStudio comes with some nice features, like terminal, git

Log into the droplet:

- `ssh root@IP_address`
- `ssh -i ~/.ssh/id_rsa root@IP_address`
- Once logged in: message of the day
- `whoami`
- where am I: `pwd`

```bash
# root user and sudo
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git
git clone https://github.com/ABbiodiversity/nerdtalk.git
cd nerdtalk
ls
ls -la

touch test.txt
vim test.txt
```

Useful key bindings: i=interactive, esc=back, :q!=exit, :wq=save

Review a few things:

- tab for autocomplete
- up/down arrow for history (2 spaces to NOT save, e.g. passwords etc)

```bash
cd ..
cd ~
mkdir newdir
cd newdir

# look up cp, I tend to forget
man cp
# escape with q

cp ~/nerdtalk/test.txt ~/newdir/test.txt
rm ~/nerdtalk/test.txt
# rm -rf ... # force, recursive
# be careful with /dir vs / dir
# you don't want rm -rf /
# why? It is the Linux version of format c: (do you recall DOS?)
```

Reboot: `reboot`, exit and close connection: `exit`.

So what is `ssh`? It creates a secure shell tunnel on [port 22](https://www.ssh.com/ssh/port).

What is a port? You'll have to wait until next week when we'll review the HTTP/HTTPS protocols and discuss how a request and a response look like.

How do we get data from our local computer to the remote? Use [sftp](https://www.digitalocean.com/community/tutorials/how-to-use-sftp-to-securely-transfer-files-with-a-remote-server). WinSCP is a GUI for Windows (along with Putty etc, it is quite useful, IMO better than FileZilla).

```bash
sftp root@IP_address
# help
?
```

| Local  | Remote |
|--------|--------|
| `lpwd` | `pwd`  |
| `lls`  | `ls`   |
| `lcd`  | `cd`   |

```bash
get ~/newdir/test.txt
# get remoteFile localFile
# get -r someDirectory # recursive for a dir
# put localFile
# put -r localDirectory

# check space
df -h

!
# this gives a local shell (use locally available commands)
exit # takes back to sftp
```

Exit sftp, ssh back into the droplet

File system tree: https://help.ubuntu.com/community/LinuxFilesystemTreeOverview

eco and redirect the output

```bash
echo helo!
echo 'helo!' > new.txt
#vim new.txt
echo 'this is me' >> new.txt # append
#vim new.txt
echo 'bye' > new.txt
```

```
ls
ls > new.txt
ls | cat # pipe
ls 1> new.txt
ls 2> new.txt # explicit redirect
ls 1> new.txt 2> error.txt
# do you recall how R's new pipe will look like?
# %>% vs |>
```

what is output? it is a data stream: https://www.howtogeek.com/435903/what-are-stdin-stdout-and-stderr-on-linux/

0. stdin
1. stdout
2. stderr

Exit, destroy.

That's all folks.
