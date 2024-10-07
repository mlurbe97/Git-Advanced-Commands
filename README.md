# Git Advanced Commands

## Author

* Manel Lurbe Sempere (manellurbe@gmail.com)

## Readme Content

<!--ts-->
* [Content](#Readme-Content)
    1. [GIT CONFIG FILE AND TOOLS](#GIT-CONFIG-FILE-AND-TOOLS)
    2. [GIT REMOTES AND BRANCHES](#GIT-REMOTES-AND-BRANCHES)
    3. [GIT COMMIT AND PUSH TO REMOTE](#GIT-COMMIT-AND-PUSH-TO-REMOTE)
    4. [GIT LOG, REFLOG AND DIFF](#GIT-LOG,-REFLOG-AND-DIFF)
    5. [GIT SQUASH AND MERGE](#GIT-SQUASH-AND-MERGE)
    6. [GIT RESET](#GIT-RESET)
    7. [GIT REVERT AND REBASE](#GIT-REVERT-AND-REBASE)
    8. [GIT STASH](#GIT-STASH)
    9. [GIT TAGS](#GIT-TAGS)
    10. [GIT SUBMODULES](#GIT-SUBMODULES)
    11. [USEFUL LINKS](#USEFUL-LINKS)

## GIT CONFIG FILE AND TOOLS

- Select the default git editor:
    ```
    git config --global core.editor code
    ```

- Default git diff editor:
    ```
    git config --global diff.tool default-difftool
    ```

    And in the .gitconfig file add this:
    ```
    [diff]
        tool = default-difftool
    [difftool "default-difftool"]
        cmd = code --wait --diff $LOCAL $REMOTE
    ```
- Usage of git difftool:
    ```
    git difftool commithash1 commithash2
    ```
    example:
    ```
    git difftool HEAD
    ```

- Remove git diff prompt:
    ```
    git config --global difftool.prompt false
    ```

- Show only the changes:
    ```
    git status -s
    ```

- Define git default merge tool:
    ```
    git config --global merge.tool code
    
    git config --global mergetool.code.cmd "code --wait $MERGED"

    git config --global mergetool.prompt false

    git config --global mergetool.keepbackup false
    ```
    Probably you have to edit this line:
    ```
    [mergetool "code"]
        cmd = "code --wait $MERGED"
    ```
    
- Check the git config settings:
    ```
    git config --global --list
    ```

- To open git configuration:
    ```
    git config --global -e
    ```

## GIT REMOTES AND BRANCHES

- Add the remote repo to our local git repo:
    ```
    git remote add origin https://github.com/mlurbe97/GIT_Advanced_Commands.git
    ```

- Check remote operations:
    ```
    git remote -v
    ```

- List existing branches:    
    ```
    git branch -a
    ```

- Change to desired branch:
    ```
    git checkout -b origin/main
    ```

- Save git credetials, use a github token as a github password:
    ```
    git config --global credential.helper store
    ```

## GIT COMMIT AND PUSH TO REMOTE

- Add local changes:
    ```
    git add .
    ```

- See added files:
    ```
    git status
    ```

- Make a commit:
    ```
    git commit -m "Commit message here."
    ```

- List references in a local repository:
    ```
    git show-ref
    ```

- Push local changes on remote repo:
    ```
    git push -u origin main
    ```

- Rename a branch:
    ```
    git branch -m origin/main dev
    ```

- Delete a branch:
    ```
    git branch -d branchname
    ```

- Force delete a branch:
    ```
    git branch -D branchname
    ```

- Update git commit message
    ```
    git commit --amend
    ```

- Need this line in git config to publish changes:
    ```
    [core]
        editor = code -e
    ```
- Update git commit message but now with command line:
    ```
    git commit --amend -m "new message here"
    ```
- Adding a missing file to the last commit:    
    ```
    echo "SOME TEXT" >> file.txt
    git add file.txt
    git commit --amend -m "new message here"
    ```

## GIT LOG, REFLOG AND DIFF

- Show git log:
    ```
    git log
    ```
    or in one line:
    ```
    git log --oneline
    ```

- Check amed changes and go to previous changes, changes that are not in git log:
    ```
    git reflog
    ```

- Got to a specifig reflog using number:
    ```
    git reflog HEAD@{refnumber}
    ```

- Got to a specifig reflog using time ago:
    ```
    git reflog HEAD@{3.days.ago}
    git reflog HEAD@{2.weeks.ago}
    git reflog master@{1.min.ago}
    git reflog dev@{2017-08-26}
    ```

- Use diff on reflog using numbers:
    ```
    git diff HEAD@{4} HEAD@{3}
    ```
    or using the tool:
    ```
    git difftool HEAD@{4} HEAD@{3}
    ```

- Use diff using time ago:
    ```
    git diff HEAD@{4.weeks.ago} HEAD@{3.min.ago}
    ```
    or using the tool:
    ```
    git difftool master@{2.min.ago} master@{2.month.ago}
    ```

- Set the unreachable commits to be deleted on the next time the garbage collector will be executed:
    ```
    git reflog expire --expire-unreachable=now --all
    ```

- Remove none-tracked/unreachable commits with garbage collector:
    ```
    git gc --prune=now
    ```

- Update remote content and branches status:
    ```
    git fetch origin --prune
    ```
## GIT SQUASH AND MERGE

Squash and merge group some branch commits in one single commit when merge, but dosn't track the branch original commits. Deleting the feature branch will be the best option after squash and merge.

- How to proceed the deleting squash merged branch:
    ```
    git checkout master
    git branch -d featurebranchname
    git fetch origin --prune
    ```
    They will still be available in reflogs. To summarize we have to develop our feature locally (using as commits as we need), send the changes in a feature branch, squash and merge, then delete the feature branch. The main branch will contain all the feature changes in one commit. After that we will need to delete the unreachable commits using previous commands for gc.

- Set aliases global or repo local (without --global flag):
    ```
    git config --global alias.onelinegraph 'log --oneline --graph --decorate'
    git config --global alias.expireunreachablenow 'reflog expire --expire-unreachable=now --all'
    git config --global alias.gcunreachablenow 'gc --prune=now'
    ```

- Set automatic fetch prune (--prune flag always true):
    ```
    git config --global fetch.prune true
    ```

## GIT RESET

- Reset an staged files after git add:
    ```
    git reset file.txt
    ```
    for all staged files:
    ```
    git reset
    ```
 
- Using reset to go back to last commit (changes of that commit will be un-staged, the following commit will be unreachable):
    ```
    git reset commithash
    ```
- Hard reset will come back to past commit but dosn't save any modifications on un-staged state:
    ```
    git reset commithash --hard
    ```
    or using HEAD or a branchname
    ```
    git reset branchname --hard
    ```

- Clean hard reset new files un-staged:
    ```
    git clean -d -x -f
    ```
    or interactive:
    ```
    git clean -d -x -i
    ```

## GIT REVERT AND REBASE

When we try to fix a precious commit bug on the master we should go back with hard reset using a branch, solve the bug and then merge into the master. The delete the branch as we do with features.

- Revert commit, to undo something. This will reverse the commit changes and generate a new commit with this changes.
    ```
    git revert commithash
    ```

- Rebase will put all the feature commits in the master. Then delete the feature branch.
    ```
    git pull origin master
    git checkout featurebranch
    git rebase master
    git checkout master
    git merge featurebranch
    git branch -d featurebranch
    ```

- After a merge request then:
    ```
    git checkout master
    git fetch origin
    git pull origin master
    ```
- On a rebase conflict, solve problems with:
    ```
    git mergetool
    ```
    then:
    ```
    git rebase --continue
    ```

## GIT STASH

Temporarily stores (or stashes) the changes you made to the code you're working on so you can work on something else and then come back and reapply the changes later. Saving changes to stashes is convenient if you need to quickly switch contexts to something else, but you're in the middle of a code change and don't have everything ready to commit the changes. Stashes are only locally.

- Create and save a stash:
    ```
    git stash save -u stashdescription
    ```

- See current stashes:
    ```
    git stash -a
    ```
    or
    ```
    git stash list
    ```

- See changes on the stash:
    ```
    git stash show stash@{stashnumber}
    ```

- Pull stash modifications:
    ```
    git stash apply stash@{stashnumber}
    ```

- Revert stash modifications:
    ```
    git stash pop stash@{stashnumber}
    ```

- Put the stash into a new branch:
    ```
    git stash branch branchname stash@{stashnumber}
    ```

- Put the branch into a new stash:
    ```
    git stash save -u branchname
    ```

- Delete a stash forever:
    ```
    git stash drop stash@{stashnumber}
    ```

- Delete all stashes:
    ```
    git stash clear
    ```

## GIT TAGS

- List all repo tags:
    ```
    git tag
    ```
    or
    ```
    git tag -l
    ```

- Create lightweight tags at current commit:
    ```
    git tag tagname
    ```

- Create tags with messages:
    ```
    git tag -a tagname -m "annotation message"
    ```

- Show tag commit info:
    ```
    git show tagname
    ```
- Search tags with regular expressions:
    ```
    git tag -l "expresion*"
    ```

- Add a past commit tag:
    ```
    git tag -a tagname -m "annotation message" commithash
    ```

- Delete an existing tag:
    ```
    git tag -d tagname
    ```

- Push tags to remote:
    ```
    git push --tags
    ```

- Remove tags on remote, first delete local and the push:
    ```
    git tag -d tagname
    git push origin :tagname
    ```

- Automatic push tags to remote whan make pushes:
    ```
    git config --global push.followTags true
    ```

## GIT SUBMODULES

- Initialize submodules of the repository:
    ```
    git submodule update --recursive --init
    ```

- Pull submodules:
    ```
    git submodule update --recursive --remote
    ```
    or:
    ```
    git pull --recurse-submodules
    ```

- Remove a submodule:
    - Delete the relevant section from the .gitmodules file.
    - Stage the .gitmodules changes:
        ```
        git add .gitmodules
        ```
    - Delete the relevant section from .git/config.
    - Remove the submodule files from the working tree and index (no trailing slash):
        ```
        git rm --cached path_to_submodule
        ```
    - Remove the submodule's .git directory:
        ```
        rm -rf .git/modules/path_to_submodule
        ```
    - Commit the changes:
        ```
        git commit -m "Removed submodule <name>"
        ```
    - Delete the now untracked submodule files:
        ```
        rm -rf path_to_submodule
        ```
        
## USEFUL LINKS

https://onlywei.github.io/explain-git-with-d3/

https://github.com/majorguidancesolutions-team/ultimate-default-web

https://github.com/Readify/GitViz

