# Git

"At the heart of GitHub is an open source version control system (VCS) called Git. Git is responsible for everything GitHub-related that happens locally on your computer." - [github docs](https://docs.github.com/en/get-started/quickstart/set-up-git)

## Getting Started

### Install git locally
**Ubuntu**
```bash
apt install git
```

**MacOS**
```bash
git # if cmd does not exist follow prompt to install
```

**Windows**
Download installer from [git downloads page](https://git-scm.com/download/win).

### Authenticating Github/Gitlab for ssh
Following [this](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) guide for generating ssh keys and adding it to the ssh-agent.

Generate ssh key
```bash
ssh-keygen -t ed25519 -C "<youremail@gmail.com>"
```
Follow prompt, then check for existing ssh keys
```bash
eval "$(ssh-agent -s)"
```
Add your ssh private key to the ssh-agent
```bash
ssh-add ~/.ssh/id_ed25519
```

To understand private/public keys more for authentication and security checkout [ledger.com](https://www.ledger.com/academy/blockchain/what-are-public-keys-and-private-keys).

## Branching
Often, companies and open-source projects will have one or two main branches. If multiple, then there would be a couple staging branches like lle or hle, and then one production branch (the customer facing code). To make changes on a lower level branch, you would often want to open a feature branch.

If you've already made changes without opening the branch:
```bash
git branch -M <feature-branch-name>
```

If you haven't made changes:
```bash
git status # check to see what branch you're currently on
git checkout <lle or dev branch name> # switch to branch you plan to build off
git checkout -b <feature-branch-name>
```

After making the changes and testing them, you'll want to add them to a commit:
```bash
git status # always check what changes you have or have not staged
git add . # this will add all files to be staged in commit but its a better practice to add specific files
git commit -m "<message describing changes>"
```

Pushing changes to feature branch:
```bash
git push origin <feature-branch-name>
```

## Feature branch naming conventions
It's a good practice to have feature branch names related to Jira stories or feature stories. Let's say feature stories have the id GREEN-####. Then I would want my feature branches to look like GREEN-####.

When merging a feature branch to lle or hle or dev, you would want the merge request title to look like *[GREEN-####][Add/Change/Remove] <description of changes>*. Sometimes, you'll have more unique situations like adhoc fixes, bug fixes, or hotfixes, which would look similar like so *[Hotfix][Add/Change/Remove] <description of change or fix being made>*.
