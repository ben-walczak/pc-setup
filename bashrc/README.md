# bashrc

**What and Why?**
.bash_profile and .bashrc helps you customize your terminal to help you to be more productive. You can do things like change colors, create aliases, git prompt, auto-completition, and export certain variables.

.bash_profile is for login shell and ssh login
.bashrc is for non-login shells

Bash profiles and dot files are essentially bash scripting, setup scripts, prompt scripts, etc.

## Setup
Change directory to home directory and create files
```bash
cd ~/.
touch .bash_profile
touch .bashrc
```

## Customizations

username@hostname directory/>
```bash
PS1="\u@\h \w/> ";
export PS1;
```

username@hostname directory/> with custom colors
```bash
PS1="$(tput setaf 166)\u$(tput setaf 116)@$(tput setaf 222)\h$(tput setaf 123) \w$(tput setaf sgr0)/> ";
export PS1;
```

seperate by components
```bash
# orange user
PS1="\[$(tput setaf 166)\]\u";
# yellow host
PS1+="\[$(tput setaf 228)\]@\h";
# blue directory
PS1+="\[$(tput setaf 71)\]@\w";
# reset color
PS1+="$(tput sgr0)";
export PS1;
```

save vars and utilize later
```bash
orange=$(tput setaf 166);
bold=$(tput bold);
reset=$(tput sgr0);

PS1="\[${bold}\]\u>";
PS1="\[${reset}\]";
export PS1;
```

## Colors

Utilizes 256 color chart
```bash
echo "$(tput setaf 166) tThis is orange"
```

Reset color
```bash
$(tput sgr0)
```

## Dotfiles
Check out [github.io](https://dotfiles.github.io/inspiration/) for some popular dotfiles or bash profiles to use for your terminal.

Very good dotfiles to mimic:
- https://github.com/cowboy/dotfiles#readme
- https://github.com/mathiasbynens/dotfiles

## Special Characters

```bash
\h  the hostname up to the first .
\s  the name of the shell
\t  the current time in 24-hour format
\u  the username of the current user
\w  the current working directory
\W  the basename of the current working directory
```