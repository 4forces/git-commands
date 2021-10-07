# Git and CLI Commands (from CI LMS)

## Git Commands

### GENERAL STEPS
1. git add
   - `git add .`: add all files that has been changed
2. `git commit -m "message"`
3. `git push`

### STAGING
1. `git add hello.txt`: stages hello.txt

### REMOVE FROM STAGING
1. `git restore --staged password.txt` as suggested in git bash. More [here](https://www.git-tower.com/learn/git/commands/git-restore/)
2. `git rm --cached password.txt`: remove password.txt from staging
- [Difference of "git rm --cached x” vs “git reset head -- x](https://stackoverflow.com/questions/5798930/git-rm-cached-x-vs-git-reset-head-x/5800164#5800164)


### SHOW DIFFERENCE BEFORE AND AFTER STATE
1. `git diff story.txt`: shows the difference between the "before" abd "after" state of story.txt file.

### SHOW COMMIT HISTORY
1. `git log`: shows commit history

### RETURN TO COMMIT STATE:
1. `git reset --hard f27e1c`: discard local changes and resets file to version/state at SHA "f27e1c"
great way to reset accidental commits and revert to previous versions

### AMEND LAST COMMIT
1. `git commit --amend`: "updates" the most recent commit with the amended changes in code.

### DISCARDING CHANGES
1. `git stash`: Discard all local changes, but save them for possible re-use later
2. `git checkout -- <file>`: Discarding local changes (permanently) to a file
3. `git reset --hard`: Discard all local changes to all files permanently

### STOP TRACKING FILE
1. `git rm -r --cached <filename.file>`: git removes tracking of filename.file (for good)
### REWRITING HISTORY - GIT RESET, GIT REVERT
`git reset` reverts changes by moving a branch reference (HEAD) backwards in time to an older commit - "rewriting history;" `git reset` will move a branch backwards as if the commit had never been made in the first place.
1. `git reset HEAD~3`: Reverts to 3 commits before HEAD.

While `git reset` works great for local branches, its method of "rewriting history" doesn't work for remote branches.
In order to reverse changes, and share those reversed changes with others, we need to use `git revert`
1. `git revert HEAD`:Creates a new commit that "reverses or undoes" the latest commit.

### CLONE FROM SOMEONE'S REPOSITORY
1. Open terminal (in VS Code)
2. Navigate to desired Project-folder
3. Type `git clone https://github.com/<someone's-user-name>/<repo-name>` - Files will be downloaded to project folder
4. Open project folder in VS Code
5. Rename someone's repo (origin) to 'upstream' (or a suitable name), to maintain link to orignal repo: `git remote rename origin upstream`
6. Point origin to own repo: `git remote add origin http://github.com/<own-user-name>/<repo_name>`
More [here](https://stackoverflow.com/questions/18200248/cloning-a-repo-from-someone-elses-github-and-pushing-it-to-a-repo-on-my-github)

### STEPS TO INITIALISE NEW GIT
1. Run the following commands first to set your account's default identity:
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```
2. `git init`: to create local git repo
3. `git add --all`: to add files to staging area
4. `git commit -m "initial commit"`: commit added files.
5. `git remote add origin https://github.com/4forces/repo`: to link the local to remote repo
6. `git push origin master`: to push the committed files to master branch @ remote repo
7. `git push -u origin master`: The -u flag is used to set origin as the upstream remote in your git config. As you push a branch successfully or up to date it, it adds upstream reference. Quoted from [here](https://www.interglobalmedianetwork.com/blog/2020-02-15-the-importance-and-advantage-of-git-push-u/)

### PUSHING TO A BRANCH
1. `git push --set-upstream origin <branch-name>`: Pushing to \<branch-name> of 'origin-master'. More [here](https://stackoverflow.com/questions/37770467/why-do-i-have-to-git-push-set-upstream-origin-branch)
2. Related: To push existing to new repo [here](https://stackoverflow.com/questions/5181845/git-push-existing-repo-to-a-new-and-different-remote-repo-server)

### Additional notes on git pushing
`git push` can also take optional arguments:
 ```
 //git push <remote-repo> <branch-location>
 git push origin main
 ```
This means:
* Go to branch `main` in **local** repository, get all the commits,
* go to `main` branch on **remote**, the `<branch-location>`,
* on `<remote-repo>` named `origin`, and update all commits
* i.e. `<branch-location>` (for e.g. `main`) is same branch name on both local and remote. For different branch name refer to [below](###pushing-to-a-different-branch)

By doing this, we specify where the commits *will come from* - `main` on local, and *where commits will go* - also `main`, but on the remote. Git will therefore not bother where we are checked out on.

<details><summary>Additional notes on git "origin"</summary>
In Git, "origin" is a shorthand name for the remote repository that a project was originally cloned from. More precisely, it is used instead of that original repository's URL - and thereby makes referencing much easier.

Referenced from [here](https://www.git-tower.com/learn/git/glossary/origin/#:~:text=In%20Git%2C%20%22origin%22%20is,thereby%20makes%20referencing%20much%20easier.)
</details>

### PUSHING TO A DIFFERENT BRANCH
To specify the \<source-branch> and \<destination-branch>:
```
git push origin <source-branch>:<destination-branch>
```
This is commonly referred to as a colon refspec. Refspec refers to a location that git can figure out (like the branch `main`, or even just `HEAD~1`).

If the remote branch does not exist, use the following command:
```
git push origin <source-branch>:<newBranch>
```

### PUSHING FROM "EMPTY" SOURCE
We can technnically specify *nothing* as a valid `source` for `git push` (and also `git fetch`):
```
git push origin :<destination-branch>
```
What happens is that the \<destination-branch> will be deleted the remote, since we are pushing *nothing* to it.
Compare to [### GIT FETCH "NOTHING" FROM A REMOTE](###git-fetch-"nothing"-from-a-remote), where fetching *nothing* actually creates a new branch locally.

### CREATING BRANCHES
1. `git branch <branch-name>`: Create \<branch-name> only (Does not checkout)
3. `git checkout -b <branch-name>`: Create \<branch-name> and checkout. Read more [here](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

### CHECKING CURRENT BRANCH
1. `git branch`: Shows list of branches on repo, and points to current branch

### SWITCHING BRANCH
1. `git checkout <branch1>`: Switch to \<branch1>. Read more [here](https://backlog.com/git-tutorial/branching/switch-branch/)
2. `git switch`: New version of `git checkout`

### GIT FETCH FROM A BRANCH (DIFFERENT FROM PULLING)
1. `git fetch <branch>`
2. `git checkout <branch-name>`
3. More about git fetch [here](https://www.atlassian.com/git/tutorials/syncing/git-fetch#:~:text=The%20git%20fetch%20command%20downloads,else%20has%20been%20working%20on.&text=When%20downloading%20content%20from%20a,available%20to%20accomplish%20the%20task.)

Note that `git fetch` does not change anything about your local state. *It will not update your main branch, or change anything about how your file system looks right now.*

**Main branch will not be updated to latest commit.**

If there are no arguments specified, just a pure `git fetch`, git will download all commits from the remote onto **all their respective local tracking branches**.

<details><summary>Further explanation</summary>
`git fetch` performs two main steps:
(1) Downloads the commits that the remote has but are missing from our local repository
(2) Updates where our remote branches point to (for instance, origin/main).
`git fetch` essentially brings our local representation of the remote repository into synchronization with what the actual remote repository looks like (right now).
</details>

### GIT FETCH - EXTENDED
`git fetch origin <remote-branch>`: Git will go to the \<remote-branch> on the remote, grab all the commits not present on local, and update on the respective local tracking branch accordingly.

Note that git will not update the local non-remote branches. For e.g. git updates the local tracking branch`origin branchName` on local repo, but will not update `branchName` on local rep.

This is in-line as explained previously, where `git-fetch` only downloads commits, but does not update/merge them into local branch.

### GIT FETCH "NOTHING" FROM A REMOTE
Similar to [PUSHING EMPTY SOURCE TO DESTINATION](###pushing-from-"empty"-source), there is a fetch from *nothing*:
```
git fetch origin :<destination-branch>
```
This will create a new branch locally, unlike pushing from an empty source, which actually **deletes** the remote branch.

Note that this can be applied to `git pull` too

### GIT PULL
`git pull`: Refers to the workflow of fetching remote changes **and then merging/updating them** with local branch. (Remember that `git fetch` does not merge/update remote commits, it only *downloads* remote commits, but does not update/merge the local branch to the latest commit fetched)

There are some ways to *fetch* and *merge* remote changes:
- `git cherry-pick origin main`
- `git rebase origin main`
- `git merge origin main`
- etc...

#### GIT PULL --REBASE
 `git pull --rebase`: shorthand for `git fetch` followed by `git rebase`

 **Note:** The standard `git pull` is shorthand for `git fetch` followed by `git merge`

### MERGING
To merge \<branch2> into \<branch1>, first checkout the branch to merge into (branch1), then merge the non-checked-out branch (branch2).
1. `git checkout <branch1>`
2. `git merge <branch2>`: Merge \<branch2> into current checked out branch

 Merging in Git creates a special commit that has **two unique parents.**
<details><summary>Further explanation</summary>
A commit with two parents essentially means "I want to include all the work from branch1 and branch2."
</details>

### REBASE
##### - *Another way to combine work between branches*
Move work from \<branch1>* directly onto \<branch2>, such that it looks as if these 2 features were developed sequentially, although in reality they were developed in parallel
```
git checkout <point1> //start from <point1>
git rebase <point2> //end at <point2>
```
2. If `<point1>` is the ancestor of `<point2>`, rebase from `<point1>` (which is already checked out) and add commits up to `<point2>`.
   - `<point2>` is further downstream from `<point1>`
3. If `<point2>` is the ancestor of `<point1>`, `<point2>` will be brought forward to `<point1>`
<details><summary>Further explanation</summary>
Rebasing essentially takes a set of commits, "copies" them, and plops them down somewhere else. While this sounds confusing, the advantage of rebasing is that it can be used to make a nice linear sequence of commits. The commit log / history of the repository will be a lot cleaner if only rebasing is allowed.
</details>

### CHERRY-PICK
1. `git cherry-pick <Commit1> <Commit2> <...>`: Copy a series of commits below your current (checkedout) location (HEAD).
### LINKING INITIALISED GIT TO A URL (for eg heroku)
1. `git remote add heroku https://git.heroku.com/my-new-app.git`

### DISPLAYING URL ORIGIN OF REPO
1. `git remote -v`

### ADDING FILES TO GITIGNORE
1. `echo '*.sql' >> .gitignore`

## MOVING AROUND IN GIT
**HEAD**: The "symbolic name" for the currently checked out commit -- it's essentially what commit you're currently working on top of. Normally points to a branch name.

### DETACHING **HEAD**
1. `git checkout <commit-hash>`: Detaches HEAD from working branch to \<commit>

### RELATIVE REFERENCES
1. `git checkout HEAD^` or `git checkout <branch>^`: Moving upwards one commit from HEAD or branch

1. `git checkout HEAD~3` or `git checkout <branch>~3`: Moving upwards 3 commits from HEAD or branch

### BRANCH FORCING
#### *To directly reassign a branch to a commit with the -f option*
1. `git branch -f <main> HEAD~3`: Moves (by force) the \<main> branch to three parents behind HEAD.

### GIT TAGS and GIT DESCRIBE
1. `git tag v1 <commit-1>`: Make a tag v1 at commit-1.
   - This tags commit-1 as v1.
If we leave the \<commit-1> off, git will just use whatever HEAD is at.
Note that you will go into detached HEAD state -- this is because you can't commit directly onto the v1 tag
2. `git describe <commit/branch/tag>`: If `<commit/branch/tag>` is not specifid, git just uses where HEAD is
- Returns `<tag>_<num_of_Commits>_g<commit-hash>`
   - `tag` is the closest ancestor tag in history
   - `num_of_Commits` is how many commits away from the tag
   - `<commit-hash>` is the hash of the current commit being described

### SPECIFYING PARENTS - git checkout HEAD^1
Like the `~` modifier, the `^` modifier also accepts an optional number after it.

Rather than specifying the number of generations to go back (what `~` takes), the modifier on `^` specifies which parent to go back from the commit - this `^` only applies to merged commits have multiple parents.

Git will normally follow the "first" parent upwards from a merge commit, but specifying a number with `^` will change to the "second parent" `^2`, "third parent" `^3`", etc.

1. `git checkout <commit>^3`: Checkout third parent of commit

2. `git checkout <commit>~^2^3`: Chains multiple commands, goes upwards once `~` from current location \<commit>, then to parent 2 `^2` from current location, and parent 3 `^3` from current location

### WHAT IS **REMOTE TRACKING BRANCH**?
 The connection between **main** and **origin main** is granted by the *"remote tracking"* property of branches.
 The **main branch** is set to track **origin main** -- this means there is an <ins>implied merge target</ins> and <ins>implied push destination</ins> for the main branch.

 During a clone, git creates a remote branch for every branch on the remote (for e.g. *origin main* for *main*). A local branch that tracks the currently active branch (usually *main*) on the remote is also created.

 This explains why the following command output is seen when **git cloning**:
`local branch "main" set to track remote branch "origin main"`

We can make any branch **origin main** by the following:

**Method 1**:
```
git checkout -b <any_branch> origin main
```

**Method 2**:
```
git branch -u origin main <any_branch>
```

This creates a new branch `<any_branch>` and sets this branch to track `origin main`.



---

### About Git Links
1. [The Git Fork-Branch-Pull Workflow](https://www.tomasbeuzen.com/post/git-fork-branch-pull/)
2. [Just git reset and push when you need to undo previous local commits](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-a-git-reset-and-push-to-remote-works-on-previous-local-commits)
3. [Using Git in a team: a cheatsheet](https://jameschambers.co/git-team-workflow-cheatsheet/)
4. [Learn Git Branching](https://learngitbranching.js.org/) and [solutions](https://github.com/saivittalb/learn-git-branching-solutions)

---

## Unix Command Line Interface (CLI)

1. Press `tab` to auto-fill file/directory names
1. Assign variables: `training=seasonal/summer.csv`
2. `echo $x`: prints variable `x` to console
2. `set`: Get full list of environment variables
3, Call variable value: add a `$` sign, for e.g. `$user` = repl, `$x` = 5, etc
1. `pwd`: print working directory
2. `history`: show history of past commands
    - `!5`: runs steps 5 in history list
    - `!<command>`: runs last instance of the command
    - Save last 3 lines of `history` to `steps.txt`
    ```shell
    history | tail -n 3 > steps.txt
    ```
3. Re-run commands:
```shell
# open the script to add commans:
nano dates.sh
# type the command(s) to run, save and exit, for eg:
cut -d , -f 1 seasonal/*.csv
# bash <file>.sh to run:
bash headers.sh
```
4. `$@`: Running commands with multiple files (google if unsure)
4. `cd ..` : chg directory `..` points to directory above current
   - e.g. `cd into /workspace/.pip-modules/lib/python3.7/site-packages/`: cd to 'sitepackages' folder (where packages are installed for python)
   - `cd -` : up one directory level
   -  Single dot `.`  refers to current directory, so `ls .` is the same as `ls`, and `cd .` does not appear to do anything
   - tilde `~` points to the home directory. `ls ~` will always list contents of the home directory. `cd ~` will always bring us to the home directory
5. `touch hello.txt`: create file hello.tx
   -  `echo "hello!" >> hello.txt`: write "hello!" into hello.txt
6. `>`: means redirect, for e.g. in `head -n 5 seasonal/summer.csv > top.csv`: head's output is writtein to a newly created file called top.csv
6. `|` : pipe symbol. Foe e.g. `head -n 5 seasonal/summer.csv | tail -n 3`: Use the output of the command on the left `head -n 5 seasonal/summer.csv` as the input to the command on the right `tail -n 3`.
6. `ls`: list files
   - `ls hello/` : list files in hello directory
   - `ls -l`: list files in 'list' format
   - `ls -la`: list all
   - `ls -R`: Recursive. Shows every file and directory in the current level, then everything in each sub-directory, and so on.
   - `ls -F`: Adds a `/` after each directory. Adds a `*` after every executable. This is for clearer view of directories, files and executables.
6. `rm hello.txt`: delete hello.txt
   - `rm -rf hello`: remove folder "hello" and its contents
7. `mkdir hello`: create new directory named hello
8. `cp hello.txt hello`: copy hello.txt to hello directory
   - `cp seasonal/autumn.csv seasonal/winter.csv backup`: copy autumn.csv and winter.csv into **backup** directory
   - `cp hello.txt world.txt`: copy hello.txt and rename it as world.txt
10. `mv world.txt hello`: move world.txt to hello directory
      - `mv folder1 folder2`: *moves* (changes the name of) directory **folder1** to directory **folder2**
12. `rmdir`: deletes directyory
12. `nano hello.txt`: opens hello.txt with file editor
13. `cat hello.txt`: prints contents of hello.txt to command line
      - `more hello.txt`: older version of `cat`
      - `less hello.txt`: print contents page by page
      - `less hello.txt bye.txt`, `spacebar` to scroll, `:n`: view multiple files, `:n` to go to next file, `:p` moves back to previous file, `:q` to quit
17. `head seasonal/summer.csv`: prints the first 10 lines (rows) of `summer.csv`
      - `head -n 3 seasonal/summer.csv`: prints the first 3 lines
18. `tail seasonal/summer.csv`: prints the last lines (rows) of the file
      - `tail -n +7 seasonal/summer.csv`: prints from line `7` onwards
14. `diff hello.txt world.txt`: compares both txt files
      - *1d0* = line 1 in file 1, (need to) delete, to match line 0 (in file 2)
      - *1c1* = line 1 in file 1, (need to) change, to match line 1 (in file 2)
15. `clear`: clears console screen
16. `df`: shows disk space
7. `cut -f 2-5,8 -d , values.csv` : prints "columns" of the file `values.csv` with the following parameters:
    - Select columns 2 through 5 and columns 8 in the file, which has comma as separator
    - `-f` fields, purpose is to specify columns
    - `-d` delimiter, purpose is to point to the separator used in the file
8. `grep bicuspid seasonal/winter.csv` : prints rows from winter.csv that contain "bicuspid". Some flags are:
      - `c`: print a count of matching lines rather than the lines themselves
      - `h`: do not print the names of files when searching multiple files
      - `i`: ignore case (e.g., treat "Regression" and "regression" as matches)
      - `l`: print the names of files that contain matches, not the matches
      - `n`: print line numbers for matching lines
      - `v`: invert the match, i.e., only show lines that don't match
9. `wc`: short for "word count". Prints the number of `-c`: characters; `-w`: words; `-l`: lines
10. Wildcards.
     - Asterisk `*` is the most common. Means to "match zero or more characters. For e.g. `cut -d , -f 1 seasonal/winter.csv seasonal/spring.csv seasonal/summer.csv` becomes `cut -d , -f 1 seasonal/*` or `cut -d , -f 1 seasonal/*.csv`
     - `?`: matches a single character, e.g. `201?.txt`
     - `[...]`:matches any character in the square brackets. E.g. `201[78].txt` gives `2017.txt` and `2018.txt`
     - `{...}`: match any comma-separated patterns within. E.g. `{*.txt, *.csv}` gives files ending with `.txt` and `.csv` but not ending with `.pdf`
11. `sort`: Puts data in order. Flags:
    - `-n` numerical
    - `-r` reverse
    - `-b` ignore leading blanks
    - `-f` fold case aka be case-insensitive
12. `uniq`: remove duplicated line values that is immmediately below (does not work if values are in alternate lines). `uniq -c`: counts the number of lines the specified values)
13. Loops:
The structure is:
```shell
# for …variable… in …list… ; do …body… ; done
for filetype in gif jpg png; do echo $filetype; done
```
### For CentOS
1. `ip addr show`: Shows IP addresses and related devices and info

2. `ipaddress add 192.168.1.10/24 dev eth0`: Set [ip_address]/[subnet_mask] dev [device_name]

3. `service sshd status`: Check if SSH daemon is running
    - `service sshd start`: Start SSH daemon

## References:
1. [Code Institute](https://codeinstitute.net/)
2. [DataCamp](https://www.datacamp.com/)
3. [FCC](https://www.youtube.com/watch?v=Wvf0mBNGjXY&t=3369s&ab_channel=freeCodeCamp.org)
---



