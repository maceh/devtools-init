# devtools-init
Steps to follow on any new PC/Computer that I'm setting up for my development

## Software List

 * VSCode - https://code.visualstudio.com/ (Win x64, User Installer)
 * Git - https://git-scm.com/downloads
 * PuTTY - https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
 * (Optional) SourceTree - https://www.sourcetreeapp.com/
 * (Optional) Windows Subsystem for Linux w/ Ubuntu - https://docs.microsoft.com/en-us/windows/wsl/install-win10

## Local Folder Setup

 * ~
   * .ssh
     * id_rsa
     * id_rsa.pub
     * some-privatekey.ppk
   * workspace
     * github
       * \[user/project\]
         * repo
     * bitbucket-cloud
     * bitbucket-local

## Windows Subsystem for Linux (WSL) / Ubuntu Configuration

```
sudo apt update
sudo apt ugprade

sudo apt install openssh htop git vim
```

## Setup initial SSH Keys

Applies to both Git Bash (Windows) and WSL Ubuntu - could be done once to work with both.

```
ssh-keygen -t rsa -b 4096 -o -a 100 -C "your_email@example.com"
```

Sources
 * Github
   * https://help.github.com/articles/connecting-to-github-with-ssh/
   * https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/
   * https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/
   * https://help.github.com/articles/testing-your-ssh-connection/
 * Wiki - https://en.wikipedia.org/wiki/Ssh-keygen
