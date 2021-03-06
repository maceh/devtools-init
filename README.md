# devtools-init

Steps to follow on any new PC/Computer that I'm setting up for my development

## Pre-Requisites

### Software List

* PuTTY - <https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html>
* Git w/Git Bash - <https://git-scm.com/downloads>
* (Optional) VSCode - <https://code.visualstudio.com/> (Win x64, User Installer)
* (Optional) SourceTree - <https://www.sourcetreeapp.com/>
* (Optional) Windows Subsystem for Linux w/ Ubuntu - <https://docs.microsoft.com/en-us/windows/wsl/install-win10>

### Local Folder Structure / Heirarchy

* ~
  * .ssh
    * id_rsa
    * id_rsa.pub
    * id_rsa.ppk
  * workspace
    * github
      * \[user/project\]
        * repo
    * bitbucket-cloud
    * bitbucket-local

Scripts to setup the folder structure (WSL/Bash)

```bash
USERNAME=maceh
mkdir -p "$HOME/.ssh"
mkdir -p "$HOME/workspace/github/$USERNAME/"
mkdir -p "$HOME/workspace/bitbucket-cloud/$USERNAME/"
```

### Windows Subsystem for Linux (WSL) / Ubuntu Configuration

```bash
sudo apt update
sudo apt ugprade

sudo apt install htop git vim
```

### Initial Setup Tasks

 1. Install PuTTY
 1. Install git
 1. Install VSCode

## SSH Setup and Configuration

### Generate and convert SSH Keys (Windows)

#### Setup initial SSH Key (id_rsa)

Applies to both Git Bash (Windows) and WSL Ubuntu - could be done once to work with both.

Inside Git Bash, run the following

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Follow the prompts to set a passphrase.

#### Convert id_rsa key into .ppk format

  1. Launch PuTTYGen
  1. From the top menu - Conversions > Import Key
  1. Locate the id_rsa file and import this
  1. Use the 'Save Private Key' option to save the id_rsa.ppk file to your .ssh folder

#### Setup pageant to start on Windows login

In Windows 10 Run dialog (Win+R), run

```powershell
shell:startup
```

In this folder, create a shortcut to pageeant with additional parameters to the ppk key file you want to open automatically

* Target - "C:\Program Files\PuTTY\pageant.exe" "C:\Users\USER\\.ssh\id_rsa.ppk"
* Start In - "C:\Program Files\PuTTY\"

Test this shortcut by launching it. You should get a prompt to type in your passphrase.

Sources

* Github
  * <https://help.github.com/articles/connecting-to-github-with-ssh/>
  * <https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/>
* Wiki - <https://en.wikipedia.org/wiki/Ssh-keygen>

### Integrate with online repositories

#### Adding the new SSH Key to your Bitbucket or GitHub account

Copy to clipboard (or open in a text editor)

```bash
clip < ~/.ssh/id_rsa.pub
```

Go to your account settings and under Security > SSH Keys - add the new key

Test the connection - Bitbucket

```bash
ssh -T git@bitbucket.org
```

Test the connection - GitHub

```bash
ssh -T git@github.com
```

References

* Bitbucket
  * <https://confluence.atlassian.com/bitbucket/set-up-an-ssh-key-728138079.html>
  * <https://confluence.atlassian.com/bitbucket/troubleshoot-ssh-issues-271943403.html>
* GitHub
  * <https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/>
  * <https://help.github.com/articles/testing-your-ssh-connection/>

### Usage Notes for SSH/Git (Windows)

#### Git Bash

* In Windows 10 - appears to rely on pageant. Ensure this is running and the key is loaded.

#### Command Prompt / PowerShell

* In Windows 10 - appears to rely on pageant. Ensure this is running and the key is loaded.

#### WSL Bash

* Does not appear to need an agent, it reads the id_rsa file automatically.

#### Misc

Steps for using ssh-agent - Didn't find a need for this in Windows but saving the steps anyway...

```bash
eval $(ssh-agent -s)
```

Add your key

```bash
ssh-add ~/.ssh/id_rsa
```

Check that the key is added

```bash
ssh-add -l
```

### Check your git config

View config

```bash
git config -l
```

If needed, set global user.name and user.email

```bash
git config --global user.name "FIRST_NAME LAST_NAME"
git config --global user.email "your_email@example.com"
```

### Test cloning a project

e.g. in Git Bash

```bash
git clone git@bitbucket.org:maceh/devtools-init.git
```

### Setup plink.exe + Setup GIT_SSH Environment Variable (Windows)

These steps are needed for VSCode's built in Git integration to work, without having to go into Git Bash etc.

In PowerShell, run plink and get it to trust these remote servers

32-bit PuTTY

```powershell
& 'C:\Program Files (x86)\PuTTY\plink.exe' git@github.com
& 'C:\Program Files (x86)\PuTTY\plink.exe' git@bitbucket.org
```

64-bit PuTTY

```powershell
& 'C:\Program Files\PuTTY\plink.exe' git@github.com
& 'C:\Program Files\PuTTY\plink.exe' git@bitbucket.org
```

In Windows 10, search the Start Menu for "Edit environment variables for your account"

Create a new variable called 'GIT_SSH' pointing at plink.exe from your PuTTY installation.
For example if you have 64-bit PuTTY installed, this should point at 'C:\Program Files\PuTTY\plink.exe'

## Visual Studio Code Setup

### Extensions

* Beautify (hookyqr.beautify)
* Preview (searking.preview-vscode)
* GitLens - Git Supercharged (eamodio.gitlens)
* reStructuredText (lextudio.restructuredtext)
* Shell launcher (tyriar.shell-launcher)
* shellcheck (timonwong.shellcheck)
* markdownlint (davidanson.vscode-markdownlint)

### Preferred Themes

* Material Theme (equinusocio.vsc-material-theme) - Palenight High Contrast
* Material Icon Theme (pkief.material-icon-theme) - Palenight

### Extension Configuration

#### Shellcheck (Windows)

For Windows, grab the latest x86 stable build from <https://github.com/koalaman/shellcheck>
(Alternatively <https://shellcheck.storage.googleapis.com/index.html>)

Save shellcheck to a location on your computer, which you can then configure the path to.

Inside settings.json, add the following, modifying the paths and exclusions as needed

```json
{
    "shellcheck.executablePath": "F:\\Programs\\Shellcheck\\shellcheck-latest.exe",
    "shellcheck.exclude": [
        "SC1017"
    ],
    "shellcheck.run": "onSave",
    "shellcheck.useWSL": false
}
```

Shellcheck can be configured to use the WSL version if you have it installed in WSL Bash

```bash
sudo apt install shellcheck
```

#### Shell Launcher (Windows)

Setup keyboard shortcut inside keybindings.json

```json
[{
    "key": "ctrl+shift+t",
    "command": "shellLauncher.launch"
}]
```

Hold CTRL+SHIFT+T to confirm that Shell Launcher opens

Out of the box, Shell Launcher looks in a number of default locations for the shell .exe's.

Search settings.json (User Settings) to see these default paths. These can be overwritten in the custom User Settings if you need.

Example

```json
{
  "shellLauncher.shells.windows": [
    {
      "shell": "C:\\Program Files\\Git\\bin\\bash.exe",
      "label": "Git bash"
    },
    {
      "shell": "C:\\Windows\\System32\\bash.exe",
      "label": "WSL Bash"
    },
    {
      "shell": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
      "label": "PowerShell"
    },
    {
      "shell": "C:\\Windows\\System32\\cmd.exe",
      "label": "cmd"
    }
  ]
}
```

#### GitLens

Example Config (unsure if this is auto-generated?) in settings.json

```json
{
    "gitlens.advanced.messages": {
        "suppressCommitHasNoPreviousCommitWarning": false,
        "suppressCommitNotFoundWarning": false,
        "suppressFileNotUnderSourceControlWarning": false,
        "suppressGitVersionWarning": false,
        "suppressLineUncommittedWarning": false,
        "suppressNoRepositoryWarning": false,
        "suppressResultsExplorerNotice": false,
        "suppressShowKeyBindingsNotice": true,
        "suppressUpdateNotice": false,
        "suppressWelcomeNotice": true
    },
    "gitlens.keymap": "alternate",
    "gitlens.historyExplorer.enabled": true
}
```
