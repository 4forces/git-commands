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

### DISCARDING CHANGES
1. `git stash`: Discard all local changes, but save them for possible re-use later
2. `git checkout -- <file>`: Discarding local changes (permanently) to a file
3. `git reset --hard`: Discard all local changes to all files permanently

### STOP TRACKING FILE
1. `git rm -r --cached <filename.file>`: git removes tracking of filename.file (for good)

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

### CREATING BRANCHES
1. `git branch <branch-name>`: Create \<branch-name> only (Does not checkout)
3. `git checkout -b <branch-name>`: Create \<branch-name> and checkout. Read more [here](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

### GET LATEST CHECKPOINT (branch name CHECKPOINT)
1. `git fetch upstream`
2. `git checkout checkpointX`: (where X is the checkpoint number)

### CHECKING CURRENT BRANCH
1. `git branch`: Shows list of branches on repo, and points to current branch

### SWITCHING BRANCH
1. `git checkout <branch1>`: Switch to \<branch1>. Read more [here](https://backlog.com/git-tutorial/branching/switch-branch/) 
2. `git switch`: New version of `git checkout`

### FETCHING FROM A BRANCH (DIFFERENT FROM PULLING)
1. `git fetch <branch>`
2. `git checkout <branch-name>`
3. More about git fetch [here](https://www.atlassian.com/git/tutorials/syncing/git-fetch#:~:text=The%20git%20fetch%20command%20downloads,else%20has%20been%20working%20on.&text=When%20downloading%20content%20from%20a,available%20to%20accomplish%20the%20task.)

### MERGING
To merge \<branch2> into \<branch1>, first checkout the branch to merge into (branch1), then merge the non-checked-out branch (branch2).
1. `git checkout <branch1>`
2. `git merge <branch2>`: Merge \<branch-name> into active branch
<details><summary>Further explanation</summary>
 Merging in Git creates a special commit that has two unique parents. A commit with two parents essentially means "I want to include all the work from branch1 and branch2."
</details>

### REBASE
##### - *Another way to combine work between branches*
Move work from \<branch1>* directly onto \<branch2>, such that it looks as if these 2 features were developed sequentially, although in reality they were developed in parallel
1. `git rebase <branch2>`: Rebase (from \<branch1>*[checked out]) onto \<branch2>
2. `git rebase <branch1>`: Updates \<branch2> onto (rebased) \<branch1> - Since branch2 is still "lagging behind" branch1. Since branch2 is the ancestor of branch1, Git will simply move branch2's reference forward in history to where branch1 is. 
<details><summary>Further explanation</summary>
Rebasing essentially takes a set of commits, "copies" them, and plops them down somewhere else. While this sounds confusing, the advantage of rebasing is that it can be used to make a nice linear sequence of commits. The commit log / history of the repository will be a lot cleaner if only rebasing is allowed.
</details>

### LINKING INITIALISED GIT TO A URL (for eg heroku)
1. `git remote add heroku https://git.heroku.com/my-new-app.git`

### DISPLAYING URL ORIGIN OF REPO
1. `git remote -v`

### ADDING FILES TO GITIGNORE
1. `echo '*.sql' >> .gitignore`

## Moving around in Git 
**HEAD**: The "symbolic name" for the currently checked out commit -- it's essentially what commit you're currently working on top of. Normally points to a branch name. 

### Detaching HEAD
1. `git checkout <commit-hash>`: Detaches HEAD from working branch to \<commit>

### Relative References
1. `git checkout HEAD^`or `git checkout <branch>^`: Moving upwards one commit from HEAD or branch

1. `git checkout HEAD~3`or `git checkout <branch>~3`: Moving upwards 3 commits from HEAD or branch

### Branch Forcing
#### *To directly reassign a branch to a commit with the -f option*
1. `git branch -f <main> HEAD~3`: Moves (by force) the \<main> branch to three parents behind HEAD.
---

### About Git Links
1. [The Git Fork-Branch-Pull Workflow](https://www.tomasbeuzen.com/post/git-fork-branch-pull/)
2. [Just git reset and push when you need to undo previous local commits](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-a-git-reset-and-push-to-remote-works-on-previous-local-commits)
3. [Using Git in a team: a cheatsheet](https://jameschambers.co/git-team-workflow-cheatsheet/)

---

## Unix Command Line Interface (CLI)

1. `pwd`: print working directory
2. `history`: show history of past commands
    - `!5`: runs steps 5 in history list
4. `cd ..` : chg directory
   - e.g. `cd into /workspace/.pip-modules/lib/python3.7/site-packages/`: cd to 'sitepackages' folder (where packages are installed for python)
   - `cd -`: up one directory level
5. `touch hello.txt`: create file hello.tx
   -  `echo "hello!" >> hello.txt`: write "hello!" into hello.txt
5. `ls`: list files
   - `ls hello/` : list files in hello directory
   - `ls -l`: list files in 'list' format
   - `ls -la`: list all
6. `rm hello.txt`: delete hello.txt
7. `mkdir hello`: create new directory named hello
8. `cp hello.txt hello`: copy hello.txt to hello directory
9. `cp hello.txt world.txt`: copy hello.txt and rename it as world.txt
10. `mv world.txt hello`: move world.txt to hello directory
11. `rm -rf hello`: remove folder "hello" and its contents

12. `nano hello.txt`: opens hello.txt with file editor 
13. `cat hello.txt`: prints contents of hello.txt to command line
14. `diff hello.txt world.txt`: compares both txt files
   - *1d0* = line 1 in file 1, (need to) delete, to match line 0 (in file 2)
   - *1c1* = line 1 in file 1, (need to) change, to match line 1 (in file 2)
15. `clear`: clears console screen
16. `df`: shows disk space

---


## Git commands - Part 2


