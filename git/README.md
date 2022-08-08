# Git

"At the heart of GitHub is an open source version control system (VCS) called Git. Git is responsible for everything GitHub-related that happens locally on your computer." - [github docs](https://docs.github.com/en/get-started/quickstart/set-up-git)

## Getting Started

### Install git locally
```bash
apt install git
```

### Authenticating Github/Gitlab for ssh
Following [this](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) guide for generating ssh keys and adding it to the ssh-agent.

Generate ssh key
```bash
ssh-keygen -t ed25519 -C "bewnwalczak1@gmail.com"
```
Follow prompt, then check for existing ssh keys
```bash
eval "$(ssh-agent -s)"
```
Add your ssh private key to the ssh-agent
```bash
ssh-add ~/.ssh/id_ed25519
```

## Branching

### General Idea

### In practice