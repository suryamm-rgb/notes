# Terminal Crash Course - Git & Command Line Notes

## Basic Terminal Commands

### ls
Lists files and folders.

```bash
ls
```

### ~ Home Directory

The `~` symbol represents the home directory.

```bash
cd ~
```

### start .

Open current folder in Windows.

```bash
start .
```

### open .

Open current folder on macOS.

```bash
open .
```

### ls folderName

List files inside a specific folder.

```bash
ls folderName
```

### pwd

Print current working directory.

```bash
pwd
```

### cd folderName

Open or move into a folder.

```bash
cd folderName
```

### cd ..

Move one folder back.

```bash
cd ..
```

### clear

Clear terminal screen.

```bash
clear
```

# Creating Files

## touch

Create a file:

```bash
touch file.html
```

Create multiple files:

```bash
touch red.tsx orange.py yellow.tsx
```

Create file inside folders:

```bash
touch folderName/folder/rocky.png
```

# Creating Folders

## mkdir

Create a folder:

```bash
mkdir folderName
```

Create multiple folders:

```bash
mkdir a b c
```

# Deleting Files and Folders

## rm

Delete a file permanently:

```bash
rm fileName
```

Deleted files do not go to recycle bin/trash.

## rm -rf

Delete folders and contents:

```bash
rm -rf folderName
```

Use carefully because deletion is permanent.

# Hidden Files

## ls -a

Show hidden files:

```bash
ls -a
```

# Command Summary

- ls → list files and folders
- pwd → print current location
- cd → change directory
- cd .. → go back one folder
- clear → clear terminal
- touch → create files
- mkdir → create directories
- rm → delete files
- rm -rf → delete folders
- ls -a → show hidden files
