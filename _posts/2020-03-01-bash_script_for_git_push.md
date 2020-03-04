---
layout: post
title: Bash script for git push
lang: en
categories:
    - Tips
tags:
    - memo

---

In Git, commit messages are very imoportant to avoid confused commit log in your branch.
But in some small projects that you develope alone, thinking about every commit message might be dull.

I usualy use the following script to send local data to the remote repository. Please try it.

```bash
#!/bin/bash
echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Go To .git root directory
cd ~/workspace/{project_name}

# Add all changes to git.
git add .

# Commit changes.
msg="update repo `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master
```
