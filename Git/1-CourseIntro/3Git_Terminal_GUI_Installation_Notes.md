# Git & GitHub Course Notes

## Git as a Terminal Tool

Git is primarily a terminal-based tool.

Git was created as a command-line tool. To use Git, we run various Git commands inside a Unix shell.

The command line is not always the most user-friendly experience, but it is at the core of Git and provides complete control.

## The Rise of Git GUIs

Companies have created graphical user interfaces (GUIs) that allow people to use Git without being command-line experts.

Popular Git GUI tools:
- GitHub Desktop
- SourceTree
- Tower
- GitKraken
- Ungit

## Git History

View commit history:

```bash
git log --oneline
```

## Git GUI Clients

### Pros
- Lower barrier of entry for beginners
- Friendlier experience
- Better visual workflow
- Useful even for developers who know command line

### Cons
- Git internals can be hidden behind automation
- Can create dependency on specific software
- Harder to fix advanced issues without command line
- Interfaces differ between GUI applications

# Installing Git on Windows

## Why Git Bash?

Bash is a command-line interface widely used by developers.

- Default shell for Linux and macOS
- Git was designed around Unix-based environments
- Windows Command Prompt is different from Unix shells

Git Bash provides a Bash-like environment on Windows and comes with Git.

## Windows Installation Steps

1. Visit the official Git website.
2. Download Git for Windows.
3. Run the installer.
4. Follow the setup wizard.
5. Keep default options unless customization is needed.
6. Complete installation.
7. Open Git Bash.
8. Verify:

```bash
git --version
```

# Installing Git on Mac

## Using Homebrew

```bash
brew install git
```

Verify:

```bash
git --version
```

## Using Installer

1. Download Git installer for macOS.
2. Open the package.
3. Follow installation steps.
4. Open Terminal.
5. Verify Git installation.

# Configuring Git

Configure your identity:

```bash
git config --global user.name "username"
git config --global user.email "email@gmail.com"
```

Other commands:

```bash
ls
git log
```

# Installing GitKraken

GitKraken is a Git GUI client recommended for learning.

Features:
- Free version available
- Visual Git workflow
- Easy repository management

## Windows

1. Download GitKraken for Windows.
2. Run installer.
3. Complete setup.
4. Open GitKraken.

## Mac

1. Download GitKraken for macOS.
2. Open downloaded file.
3. Move application to Applications folder.
4. Launch GitKraken.
