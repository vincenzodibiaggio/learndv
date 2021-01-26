---
layout: post
title:  "Multiple ssh keys to login with multiple users in multiple repos"
date:   2021-01-26 17:56:40 +0100
categories: [Tech]
tags: [ssh, git, gitlab, github]
---

# How to use multiple ssh keys to manage multiple users and repositories with git

Today, after started a new project on Gitlab, I've added my public ssh key to my Gitlab user.
For the first time I was faced with the error `Fingerprint has already been taken` because my key
was already associated to my company profile.

Now, how I can login to Gitlab with another user? 
The answer is simple: with another ssh key.

### How to generate a brand new ssh key

1. Run the command `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
2. When it ask `Enter file in which to save the key (/home/YOUR_USER/.ssh/id_rsa)` enter a new path like `/home/YOUR_USER/.ssh/id_rsa_private_profile`
3. Then add the new identity to the authentication agent with the command `ssh-add ~/.ssh/id_rsa_private_profile`. If everything will be fine, you will read a response like `Identity added: /home/YOUR_USER/.ssh/id_rsa_private_profile (/home/YOUR_USER/.ssh/id_rsa_private_profile)`

### Use the ssh key to specific repositories

To connecto to the repository using the new idendity, we should declare it to the authentication agent through the file `~/.ssh/config`:

(in this example we manage a custom identity to connect with `gitlab.com`. You can modify the value of the `HostName` key with the real hostname of your repository)

Put this text at the end of file:
```
  Host my-custom-host-name
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_rsa_private_profile
  IdentitiesOnly yes
```

Now, we just need to update (or add) the value of the git remote host with the command:

`git remote add origin git@my-custom-host-name:REAL_REPO_USER/YOUR_REPO.git`

That's it!

Bye