# OpenSSH

**What is SSH?**
SSH, also known as Secure Socket Shell, is a network protocol that gives users a secure way to access a computer over an unsecured network.

**Why OpenSSH?**
[Full Guide](https://www.youtube.com/watch?v=YS5Zh7KExvE)
- OpenSSH is a remote management tool, that gives you access to run commands on another machine
- Developed by the OpenBSD project
- It's the closest thing to a standard for remote access we have in the Linux community
- It's a suite of utilities, the most important which are the server and client components

**Explain like I'm 5, how do public/private key encryption work?**
"You could see the public key as a "lock". This lock is a very strong lock, which can't be opened except if you have a single unique key that opens it (this is the private key). If you want to send information to a person and you want to be sure that no one else can read it, you take this lock (the person hands them out to anyone who wants them) and put its on the data. From that point on, no one can open the lock any more, except for the intended recipient, who is the only one who has the private key.

...

A simple analogy is that of a locked mail box. It has a slot where you place your mail right? It's open, it's exposed, any one can touch it, any one can lick it, stick feces inside it and maybe, even put mail inside it. The street address of this mail box is it's public key (ish). Everyone knows it.

However, what stops people from reading what mail you put inside that box? You need a key to open it. Your mailman has his key, which is the private key, to open the box and take the mail. No one else has that key except him." - [Reddit](https://www.reddit.com/r/explainlikeimfive/comments/1jvduu/eli5_how_does_publicprivate_key_encryption_work/)

## Setup

```bash
sudo su
apt-get install openssh-server
apt-get install openssh-client
service ssh status
```

### Connecting to server

```bash
ssh root@172.105.7.26 ## ssh's via port 22
ls /home/<user> -a # will show ~/.ssh/known_hosts where the new port is added
```
Known hosts will prevent situations where you connect to a server you don't recognize, aka man in the middle attacks. These attacks happen when someone is intercepting your traffic and imitating your ssh session.

### Directory Tree
```bash
cd .ssh
ls
config # short cuts for ssh servers
id_ed25519 # private key
id_ed25519.pub # public key
known_hosts # trusted public keys
```

### Configuring Client

```bash
cd ~/.ssh
touch config
```

In config using `nano`, where ssh client will load this file
```
Host myserver
  Hostname 172.105.7.26
  Port 22
  User root
```

Then in bash
```bash
ssh myserver
```

Multiple servers, simply extend the list of Host servers in `config`

### Public/Private Keys

Create ssh key, add to remote server, and disable password authentication
```bash
ssh-keygen # if prompted for passphrase, you will be prompted for a password for using a key
```
*Be careful, you may overwrite a previously written key here*

.pub keys are public keys

*adding private key on ssh server helps authenticate user via private key*

In remote server
```bash
mkdir .ssh
cd .ssh
touch authorized_keys
```

In authorized_keys file paste the contents of the public key here, one line per one key

Connect to remote server via verbose
```bash
ssh -v root@172.105.7.26
```

```bash
ssh-copy-id -i ~/..ssh/id_rsa.pub root@172.105.7.26
```

### How to manage ssh keys

Generate ssh key specific to client
```bash
ssh-keygen -t ed25519 -C "<your comment here>" # more secure than default rsa and public key will be noticeably shorter
# comment has no baring on keys, defaults to username@machine
# in prompt /home/jay/.ssh/acme_id_ed25519    < this is the custom filename specified
# create passphrase for key, highly recommended bc key can fall into wrong hands
```

Copy key to server
```bash
ssh-copy-id -i ~/.ssh/acme_id_ed244p98q4098ru098
```

How do we want to differentiate which key to use for an ssh server?
```bash
ssh -i ~/.ssh/acme_id_ed25519 root@172.105.7.26
```

Caching passphrase in memory
```bash
eval "$(ssh-agent)" # if ssh-agent is not running
ps aux | grep ssh-agent
ssh-add ~/.ssh/acme_id_ed25519 # cache private key
```
- started ssh-agen in background
- added the key to the agent
- unlock the key and store it in memory

*Now I will never have to add the ssh key again*

### Configuring OpenSSH on server

```bash
which sshd # service or daemon that runs in the background
systemctl status sshd # see that service running
systemctl restart ssshd # restart service (stop/start also works in place of restart)
systemctl enable ssh # start sshd at start or boot
```

What are host keys?
- They are the keys created after the client connects to the server for the first time (the fingerprint)
- Do NOT delete these

`sshd` listens for connections; therefore, the configuration for this would determine how authentication works

Disable password authentication
In file `/etc/ssh/sshd_config`
```bash
nano /etc/ssh/sshd_config
# in file change the following lines to no
PermitRootLogin no
PasswordAuthentication no # MOST IMPORTANT: Change to no to disable tunnelled clear text passwords
```
Uncomment these lines, and, if needed, change yes to no.

Then run the following to apply the changes
```bash
service ssh restart
```

### Troubleshooting

Check log files in server
```bash
cd /var/log
tail auth.log
```

Journalctl should have log files
```bash
journalctl -u ssh # sometimes sshd
```

## Setting up Windows client

### PuTTYgen

Puttygen is a key generator tool for creating pairs of public and private SSH keys. These keys can then be used in the ntetworking client PuTTY.

To download PuTTYgen go to their [official website](https://www.puttygen.com/).

How to use PuTTYgen?
1. Once you install the PuTTY on your machine, you can easily run PuTTYgen. For the same, go to Windows -> Start Menu -> All Programs -> PuTTY -> PuTTYgen.
2. You will see the PuTTY key generator dialog box on your screen
3. You will find a “Generate” button in that dialog. Clicking on it will lead to generating the keys for you.
4. Now you will need to add a unique key passphrase in the Key passphrase and Confirm passphrase field.
5. Click on the “Save Public Key” and “Save Private Key” buttons to save your public and private keys.
6. You will see the text starting with ssh-RSA in the Public key for pasting into OpenSSH authorized_keys file field which is located at the top of the window. Copy that entire text to your clipboard by pressing ctrl+c as you will require the key to paste on your clipboard in the public key tool of control panel or directly on the cloud server.

### PuTTY

To download PuTTY go to their [official website](https://www.putty.org/)


## Network Chuck

Automatic Updates
```bash
apt update
apt-dist upgrade
apt install unattended-upgrades
dpkg-reconfigure --priority=low unadttended-upgrades
```


