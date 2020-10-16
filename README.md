# Git and CLI Commands (from Code Institute LMS)

## Git Commands

### GENERAL STEPS
1. git add
   - `git add .`: add all files that has been changed
2. `git commit -m "message"`
3. `git push`

### STAGING
1. `git add hello.txt`: stages hello.txt

### REMOVE FROM STAGING
1. `git rm --cached password.txt`: remove password.txt from staging

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

### STEPS TO INITIALISE NEW GIT
1. `git init`: to create local git repo
2. `git add --all`: to add files to staging area
3. `git commit -m "initial commit"`: commit added files.
4. `git remote add origin https://github.com/4forces/repo`: to link the local to remote repo
5. `git push origin master`: to push the committed files to master branch @ remote repo

### LINKING INITIALISED GIT TO A URL (for eg heroku)
1. `git remote add heroku https://git.heroku.com/my-new-app.git`

### DISPLAYING URL ORIGIN OF REPO
1. `git remote -v`

### ADDING FILES TO GITIGNORE
1. `echo '*.sql' >> .gitignore`

---

## Unix Command Line Interface (CLI)

1. `pwd`: print working directory
2. `history`: show history of past commands
    - `!5`: runs steps 5 in history list
4. `cd ..` : chg directory

5. `touch hello.txt`: create file hello.tx
   -  `echo "hello!" >> hello.txt`: write "hello!" into hello.txt
5. `ls`: list files
   - `ls hello/` : list files in hello directory
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

---

<img src="https://codeinstitute.s3.amazonaws.com/fullstack/ci_logo_small.png" style="margin: 0;">

Welcome 4forces,

This is the Code Institute student template for Gitpod. We have preinstalled all of the tools you need to get started. You can safely delete this README.md file, or change it for your own project.

## Gitpod Reminders

To run a frontend (HTML, CSS, Javascript only) application in Gitpod, in the terminal, type:

`python3 -m http.server`

A blue button should appear to click: *Make Public*,

Another blue button should appear to click: *Open Browser*.

To run a backend Python file, type `python3 app.py`, if your Python file is named `app.py` of course.

A blue button should appear to click: *Make Public*,

Another blue button should appear to click: *Open Browser*.

In Gitpod you have superuser security privileges by default. Therefore you do not need to use the `sudo` (superuser do) command in the bash terminal in any of the backend lessons.

## Updates Since The Instructional Video

We continually tweak and adjust this template to help give you the best experience. Here are the updates since the original video was made:

**April 16 2020:** The template now automatically installs MySQL instead of relying on the Gitpod MySQL image. The message about a Python linter not being installed has been dealt with, and the set-up files are now hidden in the Gitpod file explorer.

**April 13 2020:** Added the _Prettier_ code beautifier extension instead of the code formatter built-in to Gitpod.

**February 2020:** The initialisation files now _do not_ auto-delete. They will remain in your project. You can safely ignore them. They just make sure that your workspace is configured correctly each time you open it. It will also prevent the Gitpod configuration popup from appearing.

**December 2019:** Added Eventyret's Bootstrap 4 extension. Type `!bscdn` in a HTML file to add the Bootstrap boilerplate. Check out the <a href="https://github.com/Eventyret/vscode-bcdn" target="_blank">README.md file at the official repo</a> for more options.

--------

Happy coding!
