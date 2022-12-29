# bash

Everything about bash here

**What and Why?**
.bash_profile and .bashrc helps you customize your terminal to help you to be more productive. You can do things like change colors, create aliases, git prompt, auto-completition, and export certain variables.

.bash_profile is for login shell and ssh login
.bashrc is for non-login shells

Bash profiles and dot files are essentially bash scripting, setup scripts, prompt scripts, etc.

## .bashrc

Append .bashrc with the following if `tput` works:
```
export PS1="$(tput setaf 148)[\t]$(tput setaf 149)\u$(tput setaf 150)@$(tput setaf 151)\h:$(tput 152)\w\n $(tput setaf 153)$ $(tput setaf 15)"
```

Append .bashrc with the following when `tput` does not work:
```
export PS1="\e[1;33m\u\e[1;31m@\e[1;32m\h\e[1;36m:\w\n\e[1;35m$ \e[1;0m"
```
- Note: I'm still working out some bugs with this when command extends past window length

Aliases I always forget:
```
alias move='tar -xvf'
alias kubectl='minikube kubectl --'
```

## .bash_profile

## Setting up OpenSSH with OpenVPN
More research need to be done here as this requires more work than I would like to commit at the moment. Here are some of the resources that I'm looking at:
- [Ubuntu docs on OpenVPN](https://ubuntu.com/server/docs/service-openvpn)
- [Ubuntu docs on OpenSSH](https://ubuntu.com/server/docs/service-openssh)
- [Tech blog on installing both](https://eugenegrechko.com/blog/Installing-OpenVPN-on-Ubuntu-18.04-Server-with-OpenSSH-1.1.0)

There are a lot of security risks implementing this, so I want to take my time here and implement it correctly on the first attempt...