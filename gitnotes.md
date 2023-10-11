using git and github
what is git?
download and install git.

using git
configure git
git config --global [user.name](http://user.name/) "Name"
git config --global user.email "email"
git config --global init.default branch main

git help <command>
git <command> help

git init
git status

git add <filename>
git status

untrack a file
git rm --cached <filename>

touch .gitignore

# ignore all .txt files

- .txt

track all files
git add --all | git add -A | git add .
git status

commit: committing is like taking a snapshot of your repository at this point in time. what all your files look like right now. or its like writting an entry into a history book if you ever wanted to go back to this point in the future you could do that.

git commit -m "Your Message"
git status

to see what is been modified
git diff - this will shows what the differences are.

Staging - is a place where your files sit until you're ready to commit them.

In git we've 3 different environments

1. Working files
2. Staging
3. Commit

Remove file from staging
git restore --staged <filename>

Commit Staging files and working files at one time
git commit -a -m "Your message"

delete a file in git
git rm "<filename>"

restore deleted file
git restore "<filename>"

Rename a file
git mv "old filename" "new filename"

Review commits we made.
git log
git log --oneline

edit commit message
git commit -m "New message here" --amend
git log -p

jump to specific commit
git reset <idofcommit>

you also have the option to modify what appears in the history book and also order in which commits appear.

git rebase -i --root

Branch - another branch is a copy of main branch
git branch SecondBranch
git branch
git switch <BranchName>
git branch
git switch master

Merge branches
git merge -m "Merge message" <branchName>

delete a branch
git branch -d <branchName>
git branch

Merge conflicts
if you create a branch and you try to merge back in but main branch has changed since you created your branch in that case you will have merge conflicts.
git switch -c <Branch>
git branch

create a repository on github

push code to a github repo
git remote add origin <url>
git branch -M main|master
git push -u origin main|master

push all branches
git push --all

Pull Requests - When start a pull request your changes needs to be approved. the has to review by someone before it has merge to the main branch.

git fetch && git merge
git pull - commines fetch and merge in one command.