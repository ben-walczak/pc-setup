# OpenVPN

**What is VPN?**

**Why OpenVPN?**

## Setup

### Install OpenVPN on Ubuntu according to [Tony](https://tonyteaches.tech/openvpn-ubuntu/)

Make sure packages are up to date
```bash
apt update
apt upgrade
```

Install dependencies
```bash
apt install ca-certificates wget net-tools gnupg
```

Add OpenVPN server to your repository list
```bash
wget -qO - https://as-repository.openvpn.net/as-repo-public.gpg | apt-key add -
echo "deb http://as-repository.openvpn.net/as/debian focal main">/etc/apt/sources.list.d/openvpn-as-repo.list
apt update
```

Install OpenVPN access server
```bash
apt install openvpn-as
```

### Access the Admin Dashboard

You can access the OpenVPN administrator dashboard at https://<your-ip>:943/admin or https://<your-domain>:943/admin. In either case, the default username is openvpn.

You will need to set the password for the openvpn user. You can do this with the passwd command.

```bash
passwd openvpn
```

### OpenVPN Client Setup

In the admin dashboard, you can add users under User Management. Users can access the OpenVPN server at https://<your-ip>:943 or https://<your-domain>:943 where they can login and download the client software for their device. Supported operating systems include Mac, Windows, iOS, Android, and Linux.
