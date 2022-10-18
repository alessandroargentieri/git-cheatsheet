# git-cheatsheet
My personal git cheatsheet, gathered in years of usage. Enjoy!

```bash
# fetching all tags, branches, pruned branches
git fetch -tpf

# check which files are untracked, which are in stage, which have been modified, 
# what is the current state comparing with the respective remote branch
git status

# add files to stage
git add /path/to/file1 /path/to/file2

# add all modified and untracked files to stage
git add .

# restore staged files
$ git restore --staged /path/to/file1 /path/to/file2

# restore all staged files
$ git restore --staged .

# add to stage only the modified files (not the untracked ones)
$ git ls-files --modified | xargs git add

# commit staged files
$ git commit -m "message"

# add all files to stage and commit
$ git commit -am "message"

# push new commits to a branch
$ git push origin mybranch

# pusha new commits creating a branch on the remote and setting the upstream
$ git push -u origin mybranch

# push new commits to existing remote branch after having set the upstream
$ git push

# push of a local branch without locally switching on it
(master) $ git push origin develop:develop

# push of a local branch on a different existing remote one, without locally switching on it
(master) $ git push origin develop:otherbranch

# push of a local branch on a different remote branch even if it doesn't exist yet, without locally switching to it.
(master) git push -f origin branch1:refs/heads/branch2

# push of a remote branch on a different remote branch even if it doesn't exist yet, without locally switching to it.
(master) git push -f origin origin/branch1:refs/heads/branch2

# push remote branch1 to remote branch2 (even it branch2 doesn't exist)
$ git checkout origin/branch1 && \
git push -f origin HEAD:refs/heads/branch2

# pull new commits from the respective remote branch
$ git pull

# hey this is a merge with the remote otherbranch!
$ git pull origin otherbranch 

# hey this is a rebase with the remote branch
$ git pull --rebase

# hey this is a rebase with the remote branch for whose possible conflicts we accept the other branch solutions
$ (mybranch) git pull --rebase origin theirbranch --strategy-option theirs

# hey this is a rebase with the remote branch for whose possible conflicts we accept our branch solutions
$ (mybranch) git pull --rebase origin theirbranch --strategy-option ours

# hey is this a rebase onto?
$ git pull origin otherbranch --rebase

# create a new local branch from the current one and switch on it
$ git checkout -b mybranch
# or
$ git switch -c mybranch

# create a new local branch from the specified one (local)
$ git checkout -b to-branch from-branch
# or
$ git switch -c to-branch from-branch

# create a new local branch from the specified one (remote)
$ git checkout -b to-branch origin/from-branch
# or
$ git switch -c to-branch origin/from-branch

# switching on an existing local branch
$ git checkout mybranch 

# switching on a remote branch (detached state: you cannot commit on it!)
$ git checkout origin/mybranch

# switching on a tag (detached state: you cannot commit on it!)
$ git checkout v1.2.3

# switching on a different commit SHA (detached state: you cannot commit on it!)
$ git checkout 5892d5feb

# reset branch to specific commit sha mantaining the differences in the stage
# and keeping the untracked files
$ git reset --soft 5892d5feb

# reset branch to specific commit sha discarding the differences in the stage
# but keeping untracked files
$ git reset --hard 5892d5feb

# reset branch to specific local branch sha discarding the differences in the stage
# but keeping untracked files
$ git reset --hard mybranch

# reset branch to specific remote branch sha discarding the differences in the stage
# but keeping untracked files
$ git fetch -tpf && \
git reset --hard origin/mybranch

# clean untracked files (reset --hard cannot)
$ git clean -df

# get the name of the current branch
$ git branch --show-current

# get the local branches
$ git branch

# get the remote branches only
$ git fetch -tpf && \
git branch -r

# get merged and unmerged remote branches
$ git branch -r --merged
$ git branch -r --no-merged

# forcing a local branch to be equal to its remote one (you must be on another branch)
$ git fetch -tpf && \
git branch -f mybranch origin/mybranch

# forcing a local branch to be equal to another remote one (you must be on another branch)
$ git fetch -tpf && \
git branch -f mybranch origin/otherbranch

# forcing a local branch to be equal to anoter local one (you must be on another branch)
$ git branch -f mybranch otherbranch

# merging a feature into local version of master
(master) $ git merge feature
# TIP:
# if you do:
# git pull origin feature
# you merge master with the remote version of feature
# avoiding to fetch  and update the local version of feature

# merging feature into local version of master while being on another branch
(otherbranch) $ git merge feature master

# merging feature into local version of master 
# avoiding fast-forward (i.e. always creates a merge commit)
(master) $ git merge feature --no-ff

# rebasing feature on LOCAL version of master
(feature) $ git rebase master

# rebasing feature on LOCAL version of master (if there are conflicts they're automatically solved accepting master changes)
(feature) $ git rebase master --strategy-option theirs

# rebasing feature on LOCAL version of master  (if there are conflicts they're automatically solved accepting feature changes)
(feature) $ git rebase master --strategy-option ours

# TIP:
# if you do:
# git pull origin master --rebase
# you rebase feature with the remote version of master
# avoiding to fetch and update the local version of master

# rebasing feature into local version of master while being on another branch
(otherbranch) $ git rebase master feature

# rebasing feature detaching from local version of oldbase 
# and attaching on local version of newbase
(feature) $ git rebase --onto newbase oldbase

# rebasing feature detaching from local version of oldbase 
# and attaching on local version of newbase
# while being on another branch
(otherbranch) $ git rebase --onto newbase oldbase feature

# create new tag
git tag v1.2.3

# push the tag to the remote
git push origin v1.2.3

# push all tags
git push --all-tags

# list of all local tags
git tag

# list of remote tags
git ls-remote --tags origin

# fetch all remote tags locally
git fetch -t

# order tags from the greater to the littler (v10, v9, v8...)
git tag -l --sort=-v:refname | grep qa3-v

# delete local branch (you must be on a different branch)
# it works only if the branch is fully merged
git branch -d mybranch

# force delete local branch (you must be on a different branch)
git branch -D mybranch

# delete remote branch
git push -d origin mybranch

# force delete local branches based on a regex
git branch | grep "<pattern>" | xargs git branch -D

# delete local tag
git tag -d v1.0.0

# delete remote tag (like it was a branch)
git push -d origin v1.0.0

# amend last commit author
git commit --amend --author="John Doe <john@doe.org>"

# amend last commit message
$ git commit --amend -m <message>

# amend author
$ git config user.email alexmawashi87@gmail.com; 
$ git config user.name "alessandroargentieri"; 
$ git commit --amend --reset-author; 

# recover a disaster: we force pushed a branch, no local or remote copy anomore!
# check what has been done
$ git reflog show
# copy the safe state commit sha
# force the current branch on the safe state
$ git reset --hard <safe-commit-sha>
$ git push -f
# if you are on another branch just do:
$ git branch -f mybranch <safe-commit-sha>
# and push force!

# current local changes
$ git show

# changes applied with the last commit
$ git diff

# files changed in the last commit
$ git diff --name-status

# changes applied between two commit
$ git diff <commit1-sha> <commit2-sha>
$ git diff <commit1-sha>..<commit2-sha>

# changes applied between two branches
$ git diff branch1 branch2
$ git diff branch1..branch2

# files changed between two commit
$ git diff --name-status <commit1-sha> <commit2-sha>

# files changed between two branches
$ git diff --name-status branch1 branch2

# changes in a file between two branches
$ git diff branch1..branch2 -- /path/to/file

# changes in a file between two commits
$ git diff <commit1-sha>..<commit2-sha> -- /path/to/file

# show parents of a merge commit
$ git rev-list --parents -n 1 <merge-commit-sha> 
$ git rev-list --parents -n 2 <merge-commit-sha> 

# show commits
$ git log

# show commits in one line as graph
$ git log --oneline --graph

# show which commits branch2 has more than branch1
$ git log --oneline branch1..branch2

# show which commits two branch differ
$ git log --oneline branch1...branch2

# show which commits <commit2-sha> has more that <commit1-sha>
$ git log --oneline <commit1-sha>..<commit2-sha>

# show which commits two commits differ
$ git log --oneline <commit1-sha>...<commit2-sha>

# look for a portion of commit message in all the branches for a specified author in a time range
$ git log --all --grep="not in Bradcumb" --author=alexmawashi87@gmail.com --after="2018-07-01" --until="2018-08-31"

# look which local branches contain a specified commit sha
$ git branch --contains <commit-sha>

# look which remote branches contain a specified commit sha
$ git branch -r --contains <commit-sha>

# list of the stashes
$ git stash list

# remove all stashes
$ git stash clear

# remove specific stash from the list
$ git stash drop stash@{n}

# stash modified files
$ git stash 

# stash modified and untracked files
$ git stash -u

# stash modified and untracked files with a message
$ git stash save "message" -u

# apply last (LIFO) stash without removing it from the list
$ git stash apply 

# apply last (LIFO) stash removing it from the list
$ git stash pop

# apply specified stash without removing it from the list
$ git stash apply stash@{n}

# apply specified stash removing it from the list
$ git stash pop stash@{n}

# take specific commits and put on the HEAD of your current branch
$ git cherry-pick <commit1-sha> <commit2-sha>

# check remotes list
$ git remote

# add remote named 'origin'
$ git remote add origin https://<Username>:<token>@github.com/<Username>/<YourRepo>.git

# get remote 'origin' url
$ git config --get remote.origin.url

# set remote 'origin' url
$ git remote set-url origin https://<Username>:<token>@github.com/<Username>/<YourRepo>.git

# check default upstream/downstream for each remote
$ git remote -v 

# check for every branch/tag the respective remote default
$ git branch -vv

# disable security check SSL
$ git config --global http.sslVerify false

# add global credentials on git
$ git config --global credential.helper store
$ git config --global user.name "alessandroargentieri"
$ git config --global user.password "AbC482hdiwdsksasdd3323e23dw"
$ git config --global user.email "alexmawashi87@gmail.com"

# reset credentials
$ git config --local --unset credential.helper
$ git config --global --unset credential.helper
$ git config --system --unset credential.helper

# get username
$ git config user.name

# get user email address
$ git config user.email

# show all git configs
$ git config --list

# show project folder git configs
$ less .git/config

# show global config file
$ less ~/.gitconfig

# add SSH key to Github
$ ssh-keygen -t rsa -b 4096 -C "alexmawashi87@gmail.com"
$ ssh-add ~/.ssh/id_rsa
# copy the public key and set into Github/settings/security/SSH keys
# add new SSH url to the project:
$ git remote set-url origin abc@***.com:path/to/repo

# add a file in the .gitignore after having pushed it
$ git rm --cached project.properties
$ git rm --cached gen/
# then commit
```
