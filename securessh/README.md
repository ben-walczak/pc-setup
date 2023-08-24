## On your Linux Machine (Ubuntu 20.04):

Install OpenSSH Server (if not installed):

```bash
sudo apt update
sudo apt install openssh-server
```

To confirm SSH server is running:
```bash
sudo systemctl status ssh
```

Make a backup of your SSH configuration file before making any changes:
```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

Edit the SSH configuration file:
```bash
sudo nano /etc/ssh/sshd_config
```

In the SSH configuration file, make the following changes for enhanced security:
- Disable root login by ensuring this line is in the file and uncommented:
```bash
PermitRootLogin no
```

- Enable key authentication and disable password authentication by ensuring these lines are in the file and uncommented:
```bash
PubkeyAuthentication yes
PasswordAuthentication no
Port 22
AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2
HostKey /etc/ssh/ssh_host_ed25519_key
```

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

- Optional: Change the port number to something other than the default 22. Just make sure the new port is open in your firewall.

Restart the SSH service:
```bash
sudo systemctl restart ssh
```

Enable ssh in firewall settings:
```bash
sudo ufw allow ssh
```

Wifi Settings
```bash
sudo nano /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
```

```
[connection]
wifi.powersave = 2
```

```bash
sudo systemctl restart NetworkManager
```

KeepAlive Options
```bash
nano etc/ssh/sshd_config
```

```
ClientAliveInterval 120
ClientAliveCountMax 3
```



## On your MacBook:

Open Terminal and generate a new SSH key pair. You can provide a passphrase for extra security or leave it empty for password-less SSH login:
```bash
ssh-keygen -t rsa -b 4096
```
This will create a new RSA key pair with 4096 bits.

The public and private keys will be stored in ~/.ssh/ directory by default. Ensure your private key (id_rsa by default) is only readable by you:
```bash
chmod 600 ~/.ssh/id_rsa
```

Now, copy your public key to your Linux machine using the ssh-copy-id command. Replace username with your username on the Linux machine and ip_address with your Linux machine's IP address:
```bash
ssh-copy-id username@ip_address
```

If you changed the default SSH port on your Linux machine, use the -p flag followed by the port number:
```bash
ssh-copy-id -p port username@ip_address
```

After entering your password, your public key will be appended to the ~/.ssh/authorized_keys file on your Linux machine.

Now, you should be able to SSH into your Linux machine from your MacBook without entering a password:
```bash
ssh username@ip_address
```

If you changed the default port, include the -p flag:
```bash
ssh -p port username@ip_address
```

This should set you up with a secure, key-based SSH connection between your MacBook and Linux machine.

Remember, if your Linux machine is behind a router and you want to access it from outside your home network, you'll need to set up port forwarding on your router. Also, if your Linux machine does not have a static IP address, you might want to use a dynamic DNS service.

For aliasing your ipaddress for easier logging in you can do the following
```
nano ~/.ssh/config
```

```
Host myserver
    HostName ipaddress
    User ben
```

## Power Management Settings on Ubuntu that may interfere with persistent connection

In Ubuntu, power management settings are typically managed through the GNOME Settings application (or another settings application, depending on the desktop environment you're using). However, some of these settings may be hidden or inaccessible through the graphical user interface. For these, you can use system configuration files or command-line utilities to adjust the settings:

1. System Suspend / Sleep / Hibernation: These settings can be configured using the systemd utility systemctl. You can also modify sleep settings directly in `/etc/systemd/logind.conf` (uncomment and change the #IdleAction and #IdleActionSec lines).
2. Hard Disk Sleep: You can adjust disk sleep settings using the hdparm command-line utility. For example, `sudo hdparm -S 120 /dev/sda` sets the `/dev/sda` drive to go to sleep after 10 minutes of inactivity.
3. USB Selective Suspend: In Ubuntu, power management for USB devices is typically handled automatically by the kernel. If you need to disable power management for a specific device, you can do so by writing 'on' to the autosuspend file for that device in `/sys/bus/usb/devices/`.
4. CPU Power Management: CPU power management settings can be adjusted using the cpufrequtils utility. You can also modify these settings directly by writing to files in the `/sys/devices/system/cpu/cpu*/cpufreq/` directory.
5. Network Adapter Power Management: For Wi-Fi devices, you can adjust the power save mode by modifying the `wifi.powersave` option in `/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf` as we discussed previously.
6. Display Power Saving: This is usually controlled by the screensaver or power management settings in your desktop environment. On a lower level, the `xset` command can be used to control screen power saving settings.

Remember that making changes to system configuration files and using system utilities requires administrator (root) access, and incorrect settings can potentially cause system instability or other problems. Always make sure to backup any configuration file before modifying it, and only change settings that you understand.