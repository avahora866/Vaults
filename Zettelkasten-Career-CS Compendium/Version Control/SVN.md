---
tags:
  - SVN
  - FORMAT
  - "#TRUNK"
---

Apache Subversion which is often abbreviated as SVN, is a software versioning and revision control system distributed under an open source license

> \[!NOTE\] Note:  
> Subversion falls under centralized version control system, meaning that it uses central server to store all files and enables team collaboration
# Repository

Conventionally, every Subversion project has trunk, tags, and branches directories directly under the project's root directory.

The trunk is a directory where all the main development happens and is usually checked out by the developers to work on the project.

The tags directory is used to store named snapshots of the project. When creating a production release, the team will tag the code that goes into the release.

# SVN - Life Cycle

The branches directory is used when you want to pursue different lines of development.

## Create Repository:

The repository is a central place where developers store all their work. Repository not only stores files, but also the history about changes. Which means it maintains a history of the changes made in the files. The 'create' operation is used to create a new repository. Most of the times this operation is done only once. When you create a new repository, your VCS will expect you to say something to identify it, such as where you want it to be created, or what name should be given to the repository.

## Checkout

'Checkout' operation is used to create a working copy from the repository. Working copy is a private workplace where developers do their changes, and later on, submit these changes to the repository.

## Update

As the name suggests, 'update' operation is used to update working copy. This operation synchronizes the working copy with the repository. As repository is shared by all the teams other developers can commit their changes and your working copy becomes older.

Let us suppose Tom and Jerry are the two developers working on a project. Both check out the latest version from the repository and start working. At this point, their working copies are completely synchronized with the repository. Jerry completes his work very efficiently and commits his changes to the repository.

Now Tom's working copy is out of date. Update operation will pull Jerry’s latest changes from the repository and will update Tom's working copy.

## Perform Changes

After the checkout, one can do various operations to perform changes. Edit is the most common operation. One can edit the existing file to add/remove contents from the file.

One can add files/directories. But immediately these files/directories do not become a part of the repository, instead they are added to the pending change-list and become a part of the repository after the commit operation.

Similarly one can delete files/directories. Delete operation immediately deletes file from the working copy, but actual deletion of the file is added to the pending change-list and changes are made to the repository after the commit operation.

'Rename' operation changes the name of the file/directory. 'Move' operation is used to move files/directories from one place to another in a repository tree.

## Review Changes

When you check out the working copy or update the working copy, then your working copy is completely synchronized with the repository. But as you do changes to your working copy, it becomes newer than the repository. And it is a good practice to review your changes before the 'commit' operation.

'Status' operation lists the modifications that have been made to the working copy. As we have mentioned before, whenever you do changes in the working copy all these changes become a part of the pending change-list. And the 'status' operation is used to see the pending change-list.

'Status' operation only provides a list of changes but not the details about them. One can use diff operation to view the details of the modifications that have been made to the working copy.

## Fix Mistakes

Let us suppose one has made changes to his working copy, but now, he wants to throw away these changes. In this situation, 'revert' operation will help. Revert operation reverts the modifications that have been made to the working copy. It is possible to revert one or more files/directories. Also it is possible to revert the whole working copy. In this case, the 'revert' operation will destroy the pending change-list and will bring the working copy back to its original state.

## Resolve Conflicts:

Conflicts can occur at the time of merging. 'Merge' operation automatically handles everything that can be done safely. Everything else is considered as conflict. For example, "hello.c" file was modified in branch and deleted in another branch. Such a situation requires a person to make the decision. The 'resolve' operation is used to help the user figure out things and to inform VCS about the ways of handling the conflicts.

## Commit Changes

'Commit' operation is used to apply changes from the working copy to the repository. This operation modifies the repository and other developers can see these changes by updating their working copy.

Before commit, one has to add files/directories to the pending change-list. This is the place where changes wait to be committed. With commit, we usually provide a log message to explain why someone made changes. This log message becomes a part of the history of the repository. Commit is an atomic operation, which means either the entire commit succeeds or it is rolled back. Users never see half-finished commit.

# Commands
## Checkout - svn checkout - svn co 
Check out a working copy from a repository. If _`PATH`_ is omitted, the basename of the URL will be used as the destination. If multiple URLs are given, each will be checked out into a subdirectory of _`PATH`_, with the name of the subdirectory being the basename of the URL.

If you wish to check out some revision other than the most recent one, you can do so by providing the `--revision` (`-r`) option to the **svn checkout** command:
```
$ svn checkout -r 2 file:///var/svn/repos/test mine
A    mine/a
Checked out revision 2.
$
```

## Branching - svn copy
Creating a branch is very simple—you make a copy of your project tree in the repository using the **svn copy** command. Since your project's source code is rooted in the `/calc/trunk` directory, it's that directory that you'll copy. Where should the new copy live? Wherever you wish. The repository location in which branches are stashed is left by Subversion as a matter of project policy. Finally, your branch will need a name to distinguish it from other branches. Once again, the name you choose is unimportant to Subversion—you can use whatever name works best for you and your team.

You may already have seen **svn copy** used to copy one file to another within a working copy. But it can also be used to do a _remote copy_—a copy that immediately results in a newly committed repository revision and for which no working copy is required at all. Just copy one URL to another:

```
$ svn copy http://svn.example.com/repos/calc/trunk \
           http://svn.example.com/repos/calc/branches/my-calc-branch \
           -m "Creating a private branch of /calc/trunk."

Committed revision 341.
$
```
This command causes a near-instantaneous commit in the repository, creating a new directory in revision 341. The new directory is a copy of `/calc/trunk`. While it's also possible to create a branch by using **svn copy** to duplicate a directory within the working copy, this technique isn't recommended. It can be quite slow, in fact! Copying a directory on the client side is a linear-time operation, in that it actually has to duplicate every file and subdirectory within that working copy directory on the local disk. Copying a directory on the server, however, is a constant-time operation, and it's the way most people create branches.

## Merging - svn merge
`svn merge [-c M[,N...] | -r N:M ...] SOURCE[@REV] [TARGET_WCPATH]`
In all three forms _`TARGET_WCPATH`_ is the working copy path that will receive the differences. If _`TARGET_WCPATH`_ is omitted, the changes are applied to the current working directory, unless the sources have identical basenames that match a file within the current working directory. In this case, the differences will be applied to that file.

### Sync and Cherrypick Merges
The first form, when used without either the `-c` or `-r` options, is called a “sync” merge and `-r 1:REV` is implied. This variant is used to merge all eligible changes to a branch from its immediate ancestor branch.

When the first form is used with the `-c` or `-r` options, this is called a “cherrypick” merge and is used to merge an explicitly defined set of changes from one branch to another - In both variants of the first form, _`SOURCE`_ in revision _`REV`_ is compared as it existed between revisions _`N`_ and _`M`_ for each revision range provided.

### Reintegrate Merges
The second form is called a “reintegrate merge” and is used to bring changes from a feature branch (_`SOURCE`_) back into the feature branch's immediate ancestor branch (_`TARGET_WCPATH`_).

#### 2-URL Merges
In the third form, called a “2-URL Merge”, the difference between _`SOURCE1`_ at revision _`N`_ and _`SOURCE2`_ at revision _`M`_ is generated and applied to _`TARGET_WCPATH`_. The revisions default to `HEAD` if omitted.

## Revert - svn revert
Reverts any local changes to a file or directory and resolves any conflicted states. **svn revert** will revert not only the contents of an item in your working copy, but also any property changes. Finally, you can use it to undo any scheduling operations that you may have performed (e.g., files scheduled for addition or deletion can be “unscheduled”).
Discard changes to a file:

```
$ svn revert foo.c
Reverted foo.c
```

If you want to revert a whole directory of files, use the `--depth=infinity` option:

```
$ svn revert --depth=infinity .
Reverted newdir/afile
Reverted foo.c
Reverted bar.txt
```

Lastly, you can undo any scheduling operations:

```
$ svn add mistake.txt whoops
A         mistake.txt
A         whoops
A         whoops/oopsie.c

$ svn revert mistake.txt whoops
Reverted mistake.txt
Reverted whoops

$ svn status
?       mistake.txt
?       whoops
```

## Status - svn status - svn stat - svn st
Print the status of working copy files and directories. With no arguments, it prints only locally modified items (no repository access). With `--show-updates` (`-u`), it adds working revision and server out-of-date information. With `--verbose` (`-v`), it prints full revision information on every item. With `--quiet` (`-q`), it prints only summary information about locally modified items.

The first seven columns in the output are each one character wide, and each column gives you information about a different aspect of each working copy item.

The first column indicates that an item was added, deleted, or otherwise changed:

## Diff - svn diff - svn di
Display the differences between two paths. You can use **svn diff** in the following ways:

- Use just **svn diff** to display local modifications in a working copy.
    
- Display the changes made to _`TARGET`_s as they are seen in _`REV`_ between two revisions. _`TARGET`_s may be all working copy paths or all _`URL`_s. If _`TARGET`_s are working copy paths, _`N`_ defaults to `BASE` and _`M`_ to the working copy; if _`TARGET`_s are _`URL`_s, _`N`_ must be specified and _`M`_ defaults to `HEAD`. The `-c M` option is equivalent to `-r N:M` where `N = M-1`. Using `-c -M` does the reverse: `-r M:N` where `N = M-1`.
    
- Display the differences between _`OLD-TGT`_ as it was seen in _`OLDREV`_ and _`NEW-TGT`_ as it was seen in _`NEWREV`_. _`PATH`_s, if given, are relative to _`OLD-TGT`_ and _`NEW-TGT`_ and restrict the output to differences for those paths. _`OLD-TGT`_ and _`NEW-TGT`_ may be working copy paths or _`URL[@REV]`_. _`NEW-TGT`_ defaults to _`OLD-TGT`_ if not specified. `-r N` makes _`OLDREV`_ default to `N`; `-r N:M` makes _`OLDREV`_ default to _`N`_ and _`NEWREV`_ default to _`M`_.

## Logging - svn log
Shows log messages from the repository. If no arguments are supplied, **svn log** shows the log messages for all files and directories inside (and including) the current working directory of your working copy. You can refine the results by specifying a path, one or more revisions, or any combination of the two. The default revision range for a local path is `BASE:1`.

If you specify a URL alone, it prints log messages for everything the URL contains. If you add paths past the URL, only messages for those paths under that URL will be printed. The default revision range for a URL is `HEAD:1`.

With `--verbose` (`-v`), **svn log** will also print all affected paths with each log message. With `--quiet` (`-q`), **svn log** will not print the log message body itself, this is compatible with `--verbose` (`-v`).

Each log message is printed just once, even if more than one of the affected paths for that revision were explicitly requested. Logs follow copy history by default. Use `--stop-on-copy` to disable this behavior, which can be useful for determining branch points.
# Update - svn update - svn up
**svn update** brings changes from the repository into your working copy. If no revision is given, it brings your working copy up to date with the `HEAD` revision. Otherwise, it synchronizes the working copy to the revision given by the `--revision` (`-r`) option. As part of the synchronization, **svn update** also removes any stale locks found in the working copy.

For each updated item, it prints a line that starts with a character reporting the action taken. These characters have the following meaning:

## SVN Patch
An **SVN patch** is a file that contains a set of differences (or "diffs") between two versions of a file or set of files in **Subversion (SVN)**, a version control system. It can be used to apply changes to a codebase or share modifications between repositories.  