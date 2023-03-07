---
title: 'Setting up Git to ignore changes on existing config files'
date: 2023-03-07T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2023/03/setting-up-git-to-ignore-changes-on-existing-config-files
categories:
  - development
tags:
  - git
  - development
  - versioncontrol
  - tools
---

Git is a powerful tool for version control and managing changes in code. One of my main issue on using it on real projects, are config files. I couldn't just add them to the .gitinore-file, because I needed them to be there, but I didn't want them to accidentally be included in any commits and screw up the working copies.

While looking for a fix to this issue, I stumbled upon an option to tell git to ignore changes on specific files: `git update index --skip-worktree <file1> <file2>`.

This command tells Git to ignore any changes made to the requested files in future commits. It is very helpful for situations exactly like mine - e.g. when you have local configuration files that should not be committed to the repository.

To use the command, run the following command:

```bash
git update-index --skip-worktree <file>
```

Replace <file> with the name of the file you want git to ignore.

Once you've marked a file like that, Git will not track any changes made to that file.
To unmark a file as skip worktree, use the following command:

```bash
git update-index --no-skip-worktree <file>
```

This will tell Git to start tracking changes to the file again.

It's important to note that marking a file as skip worktree does not prevent it from being overwritten by a checkout or merge operation. If you have made changes to a file and want to ensure that those changes are not lost, you should commit them to a separate branch or stash them before performing any operations that might overwrite them.

In conclusion, `git update-index --skip-worktree` is a useful command for managing files that should not be committed to the repository. By marking a file as skip worktree, you can ensure that Git will not track any changes made to that file in future commits. Just remember to be careful when performing checkout or merge operations to avoid accidentally overwriting your changes.