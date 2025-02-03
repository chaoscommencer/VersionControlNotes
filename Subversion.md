
# Subversion

Helpful tips and tricks for configuring and using Subversion (SVN) are provided below.

## Configuration

Global configuration changes...

_NOTE: WIP._

## Routine Usage

The following commands have more common usecases used outside of initial setup.

### Clone Repository

Clone a remote repository onto your local machine.

```shell
svn checkout [-r<revision_number>] <repository_url> [<destination_path>]
```

_NOTE: The optional \<revision_number\> parameter specifies the specific revision to be checked out._  
_NOTE: The \<repository_url\> parameter specifies the URL of the repository to be checked out._  
_NOTE: The optional \<destination_path\> parameter specifies the path into which the repository will be cloned._

### Change Branch

Change the branch of the current local repository to the specified branch.

```shell
svn switch <branch_url>
```

_NOTE: The \<branch_url\> parameter is the URL of the branch to which the local repository will switch._

### Change to Branch Revision

Change the current local branch to the specified revision.

```shell
svn up -r<revision_number>
```

_NOTE: The \<revision_number\> parameter is the revision to which the current branch of the local repository will change._

### Merge Branch

Merge the branch at the specified URL into the current local branch.

```shell
svn merge [-r<start_revision_number - 1>:<end_revision_number>] <branch_url>
```

_NOTE: The optional \<start_revision_number - 1\> parameter is the number of the revision preceding the range of revisions to be merged._  
_NOTE: The optional \<end_revision_number\> parameter is the number of the final revision to be merged._  
_NOTE: The \<branch_url\> parameter is the URL of the branch to be merged into the current local branch._  
_WARNING: Due to the way SVN manages conflict resolution during a merge, it is preferred to merge one changeset at a time.  If a conflict arises, even after resolution, SVN returns to a stable state without completing the merge of subsequent revisions._

### Create Patchset

Generate a patchset from the current collection of uncommitted changes.

```shell
svn diff > <patch_file_path>
```

_NOTE: The \<patch_file_path\> parameter is the path to the patchset file to be created._

#### Example

Generate a patchset from the current collection of uncommitted changes.

```shell
svn diff > ../myrepo_branch1_patchset_2020-01-13_1.diff
```

### Apply Patch

Apply a changeset from the specified patch file to the current local branch.

```shell
svn patch <patch_file_path>
```

_NOTE: The \<patch_file_path\> parameter is the path to the file containing the patchset to be applied._

#### Example

Apply a changeset from the specified patch file to the current local branch.

```shell
svn patch ../myrepo_branch1_patchset_2020-01-13_1.diff
```

### Update Remote Branch from Local

Update the associated remote branch with changes from the current local branch.

```shell
svn commit
```

### Update Local Branch

Update the local branch to the latest version stored on the remote server.

```shell
svn update [-r<revision_number>]
```

_NOTE: The optional \<revision_number\> parameter specified the revision on the remote to which the current local branch should be updated._

### Get Blame Information

Show the blame information for the specified targets.

```shell
svn annotate ...
```

### Remove New Files

Remove new uncommitted files from the specified path of the current branch of the local repository.

```shell
svn cleanup <path> --remove-unversioned
```

_NOTE: The \<path\> parameter is the relative path to the parent directory from which uncommitted files should be removed._  
_NOTE: This operation is applied recursively._

#### Example

Remove new uncommitted files from the current directory and any descendant directories.

```shell
svn cleanup . --remove-unversioned
```

### Undo Changes

Undo all uncommitted changes from files in the specified path of the current branch of the local repository.

```shell
svn revert --recursive <path>
```

Alternatively,

```shell
svn revert -R <path>
```

_NOTE: The \<path\> parameter is the relative path to the parent directory from which uncommitted changes should be reverted._  
_NOTE: This operation is ONLY applied recursively when the `-R`/`--recursive` argument is specified._

#### Example

Undo all uncommitted changes from files in the current directory and any descendant directories.

```shell
svn revert -R .
```

### Log Commits

Log the most-recent N commits.

```shell
svn log [--limit <N>]
```

_NOTE: The optional \<N\> parameter specifies the number of commits to which the log should be limited._

#### Example

Log the most-recent 5 commits.

```shell
svn log --limit 5
```

### Get Repository Information

Get the repository and branch URL, as well as the current revision information.

```shell
svn info
```

### Get Merge Conflicts

Get the list of files containing merge conflicts.

```shell
svn status | grep -P '^(?=.{0,6}C)'
```

_NOTE: Referenced from [StackOverflow \(https://stackoverflow.com/questions/2882786/is-there-a-command-to-list-svn-conflicts\)](https://stackoverflow.com/questions/2882786/is-there-a-command-to-list-svn-conflicts)._

### Resolve Merge Conflict

Mark the merge conflict in the specified file as resolved.

```shell
svn resolve --accept=working <file_path>
```

_NOTE: The \<file_path\> parameter is the path for the file whose conflicts should be considered resolved._

### Get Status

Get the status of the local repository (`X` means clean)

```shell
svn status
```

#### Response

The first column in the `svn status` response indicates that the corresponding item was added, deleted, or otherwised changed according to the following table.

<!-- markdownlint-disable MD037 -->
| Character | Meaning                                                                                                                                                                                                               |
| :-------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   `' '`   | No modification.                                                                                                                                                                                                      |
|    `A`    | Item is scheduled for addition.                                                                                                                                                                                       |
|    `D`    | Item is scheduled for deletion.                                                                                                                                                                                       |
|    `M`    | Item has been modified.                                                                                                                                                                                               |
|    `R`    | Item has been replaced in the working copy. This means the file was scheduled for deletion, and then a new file with the same name was scheduled for addition in its place.                                           |
|    `C`    | The contents--as opposed to the properties--of the item conflict with updates received from the remote repository.                                                                                                    |
|    `X`    | Item is present due to an `externals` definition.                                                                                                                                                                     |
|    `I`    | Item is being ignored (e.g. via the `svn:ignore` property).                                                                                                                                                           |
|    `?`    | Item is not under version control.                                                                                                                                                                                    |
|    `!`    | Item is missing (e.g. it was moved or deleted without having invoked the appropriate corresponding svn command). This may also indicate a directory is incomplete (a `checkout` or `update` command was interrupted). |
|    `~`    | Item is versioned as one kind of object (file, repository, link) but has been replaced by a different kind of object.                                                                                                 |
<!-- markdownlint-enable MD037 -->

The second column in the `svn status` response indicates the status of a file's or directory's properties.

<!-- markdownlint-disable MD037 -->
| Character | Meaning                                                                                             |
| :-------: | :-------------------------------------------------------------------------------------------------- |
|   `' '`   | No modification.                                                                                    |
|    `M`    | Properties for this item have been modified.                                                        |
|    `C`    | Properties for this item are in conflict with property updates received from the remote repository. |
<!-- markdownlint-enable MD037 -->

The third column in the `svn status` response is populated only if the working copy directory is locked (see [the section titled "Sometimes You Just Need to Clean Up"](https://svnbook.red-bean.com/en/1.6/svn.tour.cleanup.html)).

<!-- markdownlint-disable MD037 -->
 | Character | Meaning             |
 | :-------: | :------------------ |
 |   `' '`   | Item is not locked. |
 |    `L`    | Item is locked.     |
<!-- markdownlint-enable MD037 -->

The fourth column in the `svn status` response is populated only if the item is scheduled for addition-with-history.

<!-- markdownlint-disable MD037 -->
| Character | Meaning                           |
| :-------: | :-------------------------------- |
|   `' '`   | No history scheduled with commit. |
|    `+`    | History scheduled with commit.    |
<!-- markdownlint-enable MD037 -->

The fifth column in the `svn status` response is populated only if the item is switched relative to its parent (see [the section titled "Traversing Branches"](https://svnbook.red-bean.com/en/1.6/svn.branchmerge.switchwc.html)).

<!-- markdownlint-disable MD037 -->
| Character | Meaning                                  |
| :-------: | :--------------------------------------- |
|   `' '`   | Item is a child of its parent directory. |
|    `S`    | Item is switched.                        |
<!-- markdownlint-enable MD037 -->

The sixth column in the `svn status` response is populated with lock information.

<!-- markdownlint-disable MD037 -->
| Character | Meaning                                                                                                                                                                                                     |
| :-------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   `' '`   | When the `--show-updates`/`-u` argument is used, the file is not locked.  When that flag is not used, this ONLY means the file is not locked in \*this\* working copy.                                      |
|    `K`    | File is locked in this working copy.                                                                                                                                                                        |
|    `O`    | File is locked--either by another user, or in another working copy.  _This appears only when the `--show-updates`/`-u` argument is used._                                                                   |
|    `T`    | File was locked in this working copy, but the lock has been 'stolen' and is invalid. The file is currently locked in this repository.  _This appears only when the `--show-updates`/`-u` argument is used._ |
|    `B`    | File was locked in this working copy, but the lock has been 'broken' and is invalid. The file is no longer locked. _This appears only when the `--show-updates`/`-u` argument is used._                     |
<!-- markdownlint-enable MD037 -->

The seventh column in the `svn status` response is populated only if the item is the victim of a tree conflict.

<!-- markdownlint-disable MD037 -->
| Character | Meaning                                    |
| :-------: | :----------------------------------------- |
|   `' '`   | Item is not the victim of a tree conflict. |
|    `C`    | Item is the victim of a tree conflict.     |
<!-- markdownlint-enable MD037 -->

The eighth column in the `svn status` response is always blank.

The ninth column in the `svn status` response is populated (only if the `-u`/`--show-updates` argument is passed to the command) with the out-of-date information.

<!-- markdownlint-disable MD037 -->
| Character | Meaning                                            |
| :-------: | :------------------------------------------------- |
|   `' '`   | The item in your working copy is up-to-date.       |
|    `*`    | A newer revision of the item exists on the server. |
<!-- markdownlint-enable MD037 -->
