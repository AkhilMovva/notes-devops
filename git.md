# GIT

## Commands

```bash
git clone url
git pull url
git status
git add .
git commit -m "modified index.jsp"
git remote add origin url
git push origin master
```

### Start the git repository

 On the local machine:

1. `mkdir myproject`: Creates a new directory to put all the files into for your project. Replace ‘myproject’ with a simple but descriptive name for your project.
2. `cd myproject`: Change directory to your new project.
3. `git init`: Initialize your new directory to be a git repository.
4. `touch README.md`: Create an empty file in your directory named README.md. It is recommended to write up the description of your project here.
5. `git add README.md`: Add this file into your git repository. Git will begin tracking changes to this file.
6. `git commit -m “Initial commit”`: Commit the changes you have made to your repository in to git’s system, and add the comment that it is your initial commit. ALL commits require a message/comment.

### Github Actions

1. Log into GitHub
2. Click ‘New Repository’
3. On the next page, give your repository a unique name
4. Leave it public and click on 'Create Repository' at the bottom
5. On the next page, find the section for existing repositories and copy the two commands they give you to your clipboard
6. In your Terminal, run the two commands from GitHub

### Connect Local to Remote

On the local machine:

1. `git remote add origin <paste SSH Clone URL>`: Register with your local git repository that you will be distributing all changes to a remote repository.
2. `git remote -v`: Verify that your local git repository can speak/sync with the remote one.
3. `git push -u origin maste`r: Push all changes to your master branch up to your remote repository. The-u flag means that git will use this remote/local combination when you type ‘git push’ going forward (until you specify a new combination).

### Make a Branch

On the local machine:

1. git checkout -b mynewbranch: This creates a new branch and switches to it in one command.

### Merging Branches

On the local machine:

1. `git checkout master`: Switch back to master branch
2. `git merge mynewbranch`: Merge all of the differences between branch and master into the master branch. This brings all the work you have done in your branch in to the master branch
3. `git push origin master`: Push all the changes you have made to your code by merging your branch to the remote repository.
