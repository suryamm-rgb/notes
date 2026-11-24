# Creating Your First GitHub Repository

See sections:

## Option 1: Existing Local Repository

-   Create an empty GitHub repository.
-   `git remote -v`
-   `git remote add origin https://github.com/username/repository.git`
-   `git push -u origin main`

## Option 2: Start From Scratch

``` bash
git clone https://github.com/username/repository.git
cd repository
git add .
git commit -m "Initial commit"
git push -u origin main
```

## Viewing Remotes

``` bash
git remote
git remote -v
```

## Adding a Remote

``` bash
git remote add <name> <url>
```

Example:

``` bash
git remote add origin https://github.com/username/repository.git
```

## Rename/Remove Remote

``` bash
git remote rename <old> <new>
git remote remove <name>
```

## Git Push

``` bash
git push <remote> <branch>
git push origin main
```

## Push to Different Branch

``` bash
git push <remote> <local-branch>:<remote-branch>
git push origin pancake:waffle
git push origin cats:master
```

## git push -u

Sets upstream tracking.

``` bash
git push -u origin main
```

Afterwards you can use `git push` and `git pull`.
