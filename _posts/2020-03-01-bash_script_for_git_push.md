---
layout: post
title: Bash script for git push
lang: en
categories:
    - Tips
tags:
    - Git

---

<a href='https://500px.com/photo/1020520426/El-charco-de-La-Laja-Puesta-de-Sol-by-Fernando-Lopez' alt='El charco de La Laja Puesta de Sol by Fernando Lopez on 500px.com'>
  <img src='https://drscdn.500px.org/photo/1020520426/m%3D900/v2?sig=030069c9c40fd7219d4f7b87f23b0e26f2eb84e701e8e867ee04f716d7b3bef6' alt='El charco de La Laja Puesta de Sol by Fernando Lopez on 500px.com' />
</a>
<script type='text/javascript' src='https://500px.com/embed.js'></script>

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
