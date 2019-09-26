# Git advanced workaround

Git Working Directory:
![alt text]( ../images/git-workingDir.png "Git Working Directory")

## Git log ---

```
git log

git log --stat

```
--stat









## Diff Add












## Git branch


## Git stash

## Git reset
Most common git reset as follows
- soft as option with reset command
- mixed (default), no option with reset command
- hard as option with reset command

Git Commit history:
![alt text]( ../images/git-commit-history.png "Git Commit History")

- Step 1: Move HEAD
![alt text]( ../images/git-reset-soft.png "Git Reset Soft")
result: HEAD move back. However, files remain in the staging area.
In order to undo we need to execute ```git checkout .``` command

- Step 2: Updating the Index (--mixed)
![alt text]( ../images/git-reset-mixed.png "Git Reset Mixed")
result: HEAD move back. However, files remain in the working/staging area depending on previous activites. In order to undo we need to execute ```git checkout . && git clean -df``` command

- Step 3: Updating the Working Directory (--hard)
![alt text]( ../images/git-reset-hard.png "Git Reset Hard")
result: HEAD move back as well as files come back to working area. In order to undo we need to execute ```git clean -df``` command


**Summary of Git Reset**:

Command format | HEAD	| Index	| Workdir	| WD Safe?
-------|----- |-------|---------|---------
reset --soft [commit] | REF | NO | NO | YES
reset [commit] | REF | YES | NO | YES
reset --hard [commit] | REF | YES | YES | NO

Reference:
[Git Reset Demystified](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified)


## Commom mistakes & rollback

### Unmodifying a Modified File
- Files tracking condition
```
git checkout ${fileName}    // for specific file
git checkout .              // all files in the current directory
```
- Unstaging a Staged File
```
git reset ${filename}
```
- Deleting from working area
```
git clean -df             // -d for directory, -f for files
```

### Amend some works
Add some changes to previous commit or change the commint messages
```
git commit --amend -m "new commit msg"
```
**Notes: change SHA-1 code means change git commit history. It could harmful for shared/public repository**

### Move back commit to relevent branch

Describe the scenario:<br>
Master branch SHA-1 code (f30ab --> 34ac2 --> 98ca9) <br>
Feature branch SHA-1 code (         34ac2 --> 87ab2)<br>
Developer made a commit in Feature branch (87ab2). However, he needs to bring it back to master branch.

Workaround:
- git cherry-pick in the master branch (bring the commint to master branch)
- git reset to bring HEAD pointer back to previous working stage (means delete the commit)

```
git checkout ${feature}
git log         // Copy the SHA-1 code
git checkout master
git cherry-pick ${SHA-1 Code}     // Let's say, 87ab2
git checkout ${feature}
git reset ${target SHA-1 code}
git checkout . or
git clean -df           // Depending on git status
```
