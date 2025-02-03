
# Git

Helpful tips and tricks for configuring and using Git are provided below.

## Global Configuration

Global configuration changes will be applied to the user .gitconfig file (located in the home directory).

### Assign User's Name

Set the globally-scoped name to use for commit signatures.

```shell
git config --global user.name <name>
```

#### Example

Set the user's name.

```shell
git config --global user.name "John Doe"
```

### Assign Signing Email

Set the globally-scoped user email address to use for commit signatures.

```shell
git config --global user.email <email_address>
```

_NOTE: As a security measure to protect your email from spam, it is recommended to use an auto-generated no-reply email address, such as those provided through GitHub._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Set the email address.

```shell
git config --global user.email john.doe@users.noreply.github.com
```

### Assign GPG Signing Key

Set the GPG signing key repositories will use for commit sign-off and user authentication.  Additionally, enable GPG signing on repositories by default.

```shell
git config --global user.signingkey <signing_key>
git config --global commit.gpgsign true
git config --global tag.forceSignAnnotated true
git config --global tag.gpgsign true
```

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Set the 16-character code for the GPG signing key, and enable GPG signing.

```shell
git config --global user.signingkey ****************
git config --global commit.gpgsign true
git config --global tag.forceSignAnnotated true
git config --global tag.gpgsign true
```

### Rebase On Pull

Configure pull to use the rebase strategy (instead of merge).

```shell
git config --global pull.rebase true
```

_NOTE: Using rebase allows your repository to maintain a linear history._

### Update Default Branch Name

Change the default (master) branch for newly created repositories.

```shell
git config --global init.defaultBranch <new_branch_name>
```

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Update the default branch name to main (instead of master); this is the new norm--it's also shorter.

```shell
git config --global init.defaultBranch main
```

### Only Push Matching Branch

Push the current branch to its configured upstream branch \[only\] if the branch names match; fail otherwise.

```shell
git config --global push.default simple
```

### Assign Mailmap File

Assign a global mailmap file for git to use.  This file maps user names and emails to singular identities which can be more easily tracked in log history and commit blames.

```shell
git config --global mailmap.file <mailmap_file_path>
```

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Assign the git.mailmap file located in the user's `~/.ssh` directory.

```shell
git config --global mailmap.file ~/.ssh/git.mailmap
```

### Assign Editor

Assign a global editor for git to use.  The editor is used for various interactive git actions, including commit \[message\] editing and tagging as well as rebasing.  If not specified, git instead uses the system editor defined in one of the shell environment variables `VISUAL` or `EDITOR`, falling back to the vi editor if neither of those are defined.

```shell
git config --global core.editor <editor command>
```

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Assign VS Code as the default editor.

```shell
git config --global core.editor "code --wait"
```

_NOTE: Some alternatives include emacs and nano._

### Disable Auto-CRLF

Disable the auto-CRLF feature.  This feature can wreak havoc with commits if not supervised.  The default setting for Linux-based environments is to disable this feature anyways.  Assuming your editor is already configured to use LF-style line-endings, there is no need to enable the feature.

```shell
git config --global core.autocrlf false
```

_WARNING: Avoid disabling the auto-CRLF feature if you're developing within a Windows environment, AND your editor is not configured to save with LF-style line endings._

### Enable Diff Color Distinction for Moved Lines

Use distinct coloration to identify lines which have been moved, but have otherwise been left unchanged.

```shell
git config --global diff.colorMoved zebra
```

### Assign Git-Review Username

Assign the username to use when working with git-review and Gerrit.

```shell
git config --global --add gitreview.username <username>
```

_NOTE: The \<username\> parameter is the username associated with Gerrit._  
_NOTE: This configuration is only helpful when working with git-review and is otherwise unnecessary._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Assign the Gerrit username `jdoe`.

```shell
git config --global --add gitreview.username jdoe
```

## Routine Usage

The following commands have more common usecases used outside of initial setup.

### Create Branch

Create a new branch from the current branch at the specified commit.

```shell
git checkout -b new_branch_name <commit_hash>
```

_NOTE: The \<commit_hash\> parameter refers to the hash of a commit from the current branch that the new branch will start at._

### Update Branch from Remote

Update the current local branch with the most recent changes from the branch stored on the remote repository.

```shell
git pull --rebase origin <remote_branch_name>
```

_NOTE: The remote_branch_name parameter is the branch that the current branch will be rebased against._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Update the current branch with the most recent changes from the remote copy of the `main` branch.

```shell
git pull --rebase origin main
```

### Update Branch from Local - Rebase Strategy

Update the current local branch with the most recent changes from the specified local branch.

```shell
git rebase <local_source_branch_name>
```

_NOTE: The \<local_source_branch_name\> parameter is the name of the local branch that the current branch will be rebased against._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Update the current branch with changes from the local copy of the `dev` branch.

```shell
git rebase dev
```

### Update Branch from Local - Merge Strategy

Update the current local branch with the most recent changes from the specified local branch using the merge strategy.

```shell
git merge <local_source_branch_name>
```

_NOTE: The \<local_source_branch_name\> parameter is the name of the local branch that the current branch will be rebased against._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Update the current branch with changes from the local copy of the `dev` branch.

```shell
git merge dev
```

### Update Branch from Range of Commits

Update the current local branch with the specified range of commits (commit `A` included).  The current branch is updated by rebasing the specified range of commits on top of it.

```shell
git cherry-pick A^..B
```

_NOTE: The letters `A` and `B` above represent the hashes of the commits in the desired range.  They may be abbreviated hashes._

Alternatively, the following command\[s\] will cause the metadata for the new commits to retain the original authors and author\[ed\] dates:

```shell
# NOTE: This will leave the repository at a detached head
git rebase --onto <branch_name> A^ B
# NOTE: These commands will update the specified branch to the new head
git branch -f <branch_name> HEAD
git checkout <branch_name>
```

_NOTE: The \<branch_name\> parameter is the name of the branch whose HEAD will be changed._

### List Files with Conflicts

List the files with outstanding conflicts caused by an in-progress merge/rebase.

```shell
git diff --name-only --diff-filter=U
```

### Update Remote Branch from Local

Update the target remote branch with changes from the specified local branch.

```shell
git push origin <local_source_branch_name>:<remote_target_branch_name>
```

_NOTE: The \<local_source_branch_name\> parameter is the name of the local source branch from which the \<remote_target_branch_name\> remote branch will be updated._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Update the `origin`'s `main` branch with changes from the local `dev` branch.

```shell
git push origin dev:main
```

### Pair Local Branch with Remote Branch

Set the upstream remote branch for the current branch to the specified target branch.  Subsequent pushes and pulls from the current local branch will interact with the specified remote target branch.

```shell
git push -u origin <remote_target_branch_name>
```

_NOTE: The \<remote_target_branch_name\> parameter is the name of the remote target branch to which changes from the current local branch will be pushed.  Subsequent pushes and pulls from the current local branch will interact with the specified remote branch._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Set the remote upstream branch for the current local branch to main, and push changes to it.

```shell
git push -u origin main
```

### Rename Local Branch

Rename the specified local branch with the specified replacement name.

```shell
git branch -m <local_branch_name> <new_branch_name>
```

_NOTE: The \<local_branch_name\> parameter is the name of the local branch being renamed, and the \<new_branch_name\> parameter is the new name the branch is receiving._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Rename the `dev` branch to `test`.

```shell
git branch -m dev test
```

### Delete Remote Branch

Delete the specified remote branch.

```shell
git push origin --delete <remote_branch_name>
```

Alternatively, use:

```shell
git push origin :<remote_branch_name>
```

_NOTE: The \<remote_branch_name\> parameter is the name of the remote branch to be deleted._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Delete the remote `dev` and `test` branches.

```shell
git push origin --delete dev
git push origin :test
```

### Delete Local Branch

Delete the specified local branch.

```shell
git branch -d <local_branch_name>
```

_NOTE: The \<local_branch_name\> parameter is the name of the local branch to be deleted._  
_NOTE: The `-D` argument may be required (instead of `-d`) to force branch deletion if the local branch contains changes not captured in the corresponding upstream branch on the remote repository._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Delete the local `dev` branch.

```shell
git branch -d dev
```

### Log New Commits

Log the new commits on the specified branch from where it diverges from the current local branch.

```shell
git log --first-parent <branch_name>
```

_NOTE: The \<branch_name\> parameter is the name of the branch whose commits should be logged._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Log the new commits on the local `dev` branch.

```shell
git log --first-parent dev
```

### Log Commits in One Line

Log the most recent N commits, reserving one line for each commit.

```shell
git log --oneline [-<N>]
```

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Log the most recent 5 commits in one line each.

```shell
git log --oneline -5
```

### Print Number of Commits Per Author

Print the number of commits made by each author--on (all) branches.

```shell
git shortlog -sn --all
```

_NOTE: The -n argument performs a descending sort on the results._  
_NOTE: The -s argument suppresses commit description, showing only the count._

### Print Number of Commits by Author

Print the number of commits made by the specified author in the current local branch.

```shell
git shortlog -sn --author="<author_name>"
```

_NOTE: The \<author_name\> parameter is the name of the author whose commits should be counted._  
_NOTE: The git mailmap file will come in handy here._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Print the number of commits made by John Doe.

```shell
git shortlog -sn --author="John Doe"
```

### List Remote Repositories

List the remote repositories.

```shell
git remote -v
```

### Add Remote Repository

Add a remote repository to the list of remotes with the specified remote name.

```shell
git remote add <new_remote_name> <new_remote_url>
```

_NOTE: The \<new_remote_name\> parameter is the name that will be assigned to the remote within the context of the local git repository._  
_NOTE: The \<new_remote_url\> parameter is the URL of the new remote repository being added._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Add the passed repository URL to the local repository as the `origin` remote.

```shell
git remote add origin git@github.com:jekyll/jekyll.git
```

### Assign New Remote URL

Change the URL associated with a locally-configured remote repository name.

```shell
git remote set-url <remote_name> <new_remote_url>
```

_NOTE: The \<remote_name\> parameter is the locally-associated name of the remote repository whose URL will be re-assigned._  
_NOTE: The \<new_remote_url\> parameter is the new URL the remote name is to be associated with._  
_NOTE: This tends to be useful with working with forks or multiple repositories with the same base code._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Change the URL associated with the `origin` remote.

```shell
git remote set-url origin git@github.com:jekyll/jekyll.git
```

### Rename Remote

Change the name of a locally-configured remote repository.

```shell
git remote rename <remote_name> <new_remote_name>
```

_NOTE: The \<remote_name\> parameter is the locally-associated name of the remote repository._  
_NOTE: The \<new_remote_name\> parameter is the new locally-associated name for the remote repository._  
_NOTE: This tends to be useful with working with forks or multiple repositories with the same base code._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Change the name of the `origin` remote to `jekyll`.

```shell
git remote rename origin jekyll
```

### List Changed Files

List the files modified within the specified range of commits.

```shell
git show --pretty="format:" --name-only <BEGIN_COMMIT>..<END_COMMIT> | sort | uniq
```

_NOTE: The \<BEGIN_COMMIT\> parameter is the hash of the first commit in the range._  
_NOTE: The \<END_COMMIT\> parameter is the hash of the last commit in the range._  
_NOTE: The commit hashes may be abbreviated._

### Modify Commit Messages

Amend the commit messages of the most recent \<N\> commits.

```shell
git rebase -i -p HEAD~<N>
```

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Amend the commit messages of the most recent 3 commits.

```shell
git rebase -i -p HEAD~3
```

### Undo Commits

Undo the most recent \<N\> commits in the commit history for the current local branch, but retain the associated changes locally for subsequent revision.

```shell
git reset HEAD~<N> --soft
```

_NOTE: The \<N\> parameter specifies the number of commits to revert._  
_NOTE: This can be helpful when you'd like to update a preceding commit with some of those changes, such as with `commit --amend`._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Undo the most recent local commit, while retaining the changes in the local repository.

```shell
git reset HEAD~1 --soft
```

### Rearrange, Edit, and/or Drop Changes

Interactively rearrange, edit, drop, etc. commits from the current \[local\] branch.

```shell
git rebase -i -r <commit_hash>
```

_NOTE: The \<commit_hash\> parameter specifies the hash of the most recent commit that won't be included in the modifications; all newer commits will be included._  

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Interactively modify the most recent three local commits.

```shell
git rebase -i -r HEAD~3
```

### Clean Up Repository

Clean the repository of orphaned commits.

```shell
git reflog expire --expire-unreachable=now --all
git gc --prune=now
```

_NOTE: During branch modifications, deletions, resets, etc. previously reachable commits can become "orphaned"--that is they can become no longer reachable within the existing branches.  Data for those commits is retained, such that it may subsequently be recovered.  However, if those changes are no-longer needed, as is likely, than the repository can be cleaned up to recover some space and reduce its digital footprint._

### Search Commits for Text

Search commit changes for specified text.

```shell
git log -S "<text>" --source --all
```

_NOTE: The \<text\> parameter is the text to be searched for._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Search commit changes for "Hello world".

```shell
git log -S "Hello world" --source --all
```

### List Changed Files - with Status

List the files changed in a specified commit along with their status.

```shell
git show --name-status <commit_hash>
```

_NOTE: The \<commit_hash\> parameter is the hash of the commit whose files should be listed._  
_NOTE: The commit hash can be abbreviated._

### List Only Files Added

Only list the files that were added in a specified commit.

```shell
git show --name-status <commit_hash> | grep "^A"
```

Alternatively, the following should work, but doesn't seem to...

```shell
git show --name-status --porcelain <commit> | grep "^A" | cut -c 4-
```

_NOTE: The \<commit_hash\> parameter is the hash of the commit whose files should be listed._  
_NOTE: The commit hash can be abbreviated._

### Print Number of Files in Commit

List the number of files changed in a specified commit.

```shell
git log --oneline --name-status <commit_hash> -1 | wc -l
```

_NOTE: The \<commit_hash\> parameter is the hash of the commit whose files should be listed._  
_NOTE: The commit hash can be abbreviated._

### Point Branch HEAD to Commit

Change the head of a branch to the specified commit.

```shell
git checkout <another_branch_name>
git branch -f <branch_name> <commit_hash>
```

_NOTE: First must change to another local branch to proceed with the operation safely._  
_NOTE: The \<another_branch_name\> parameter is the name of another branch._  
_NOTE: The \<branch_name\> parameter is the name of the branch whose HEAD will be changed._  
_NOTE: The \<commit_hash\> parameter is the hash of the commit to which the HEAD of the \<branch_name\> branch will point._  
_NOTE: The commit hash can be abbreviated._

Alternatively, the following command\[s\] can be used to change the head of the current branch:

```shell
git checkout <branch_name>
git reset --hard <commit_hash>
```

_WARNING: The alternate method exposes your local repository to the potential loss of any unsaved/uncommitted changes._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Change the head of the `main` branch to the specified commit.

```shell
git checkout dev
git branch -f main ABCDEF
```

### Initialize and Update All Submodules

Initialize and update all submodules, recursively.

```shell
git submodule update --init --recursive
```

### Add Submodule

Add the specified repository as a submodule to the current repository.

```shell
git submodule add <remote_repository_url>
```

_NOTE: The \<remote_repository_url\> parameter is the URL of the remote repository to be added as a submodule._

<!-- markdownlint-disable MD024 -->
#### Example
<!-- markdownlint-enable MD024 -->

Add Jekyll as a submodule to the current repository.

```shell
git submodule add git@github.com:jekyll/jekyll.git
```

### Remove Submodule

Remove the specified submodule from the current repository.

```shell
# Manually delete the relevant section from the .gitmodules file.

# Stage the .gitmodules changes
git add .gitmodules

# Delete the relevant section from .git/config.

# Run the following command (with no trailing slash):
git rm --cached path_to_submodule
# Run the following command (with no trailing slash):
rm -rf .git/modules/path_to_submodule

# Commit changes
git commit -m "Removed submodule "

# Delete the now untracked submodule files
rm -rf path_to_submodule
```
