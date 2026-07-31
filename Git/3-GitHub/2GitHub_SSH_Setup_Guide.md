# GitHub SSH Setup Guide

## Configure Git Identity

``` bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Verify:

``` bash
git config --global --list
```

## Why Use SSH?

SSH lets you authenticate with GitHub without entering your username and
password every time you push or pull.

## Check for Existing SSH Keys

``` bash
ls -al ~/.ssh
```

Look for one of these public key files:

-   `id_rsa.pub`
-   `id_ecdsa.pub`
-   `id_ed25519.pub`

If one exists, you can reuse it.

## Generate a New SSH Key

``` bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Press **Enter** to accept the default file location.

Optionally enter a passphrase for extra security.

## Start the SSH Agent

``` bash
eval "$(ssh-agent -s)"
```

## Add the SSH Key

``` bash
ssh-add ~/.ssh/id_ed25519
```

## Create an SSH Config File (Optional)

``` bash
touch ~/.ssh/config
```

Open it:

``` bash
nano ~/.ssh/config
```

Add:

``` text
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes
```

Save and exit.

## Copy the Public Key

macOS:

``` bash
pbcopy < ~/.ssh/id_ed25519.pub
```

Linux:

``` bash
cat ~/.ssh/id_ed25519.pub
```

Copy the displayed key.

## Add the Key to GitHub

1.  Sign in to GitHub.
2.  Click **Profile** → **Settings**.
3.  Select **SSH and GPG keys**.
4.  Click **New SSH key**.
5.  Give the key a title.
6.  Paste the public key.
7.  Click **Add SSH key**.

## Test the Connection

``` bash
ssh -T git@github.com
```

Expected message:

``` text
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

## Clone Using SSH

``` bash
git clone git@github.com:username/repository.git
```

## Common Commands

``` bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
ls -al ~/.ssh
ssh-keygen -t ed25519 -C "your-email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
touch ~/.ssh/config
ssh -T git@github.com
```
