---
title: "Sending patches using git-email"
date: 2008-09-25
---

If you work on a project that uses git as repository, you might need a simple way to send your patches to others in a simple, clean way. For this, I've  started (for a while now) using a very cool tool called `git-send-email`.

Install `git-email` e.g. on Ubuntu Linux:

```shell
$ sudo apt-get install git-email
```

Add these options to `~/.gitconfig`:

```gitconfig
[sendemail]
  smtpserver = "yourmailserver.com"
  smtpuser = "user"
  smtppass = "abc123"
```

Now suppose you have a local patch that needs reviews. Pick up its hash with the command below:

```shell
$ git log -n2

commit a268d86e547dffe73c9f7b4633913ffdf91b1a8e
[..]
commit bf14b2f140fc84a57f61c7f59efae294b2a9a1df
[..]
```

Pick up the commit hashes and create a formal patch file using the command below:

```shell
$ git format-patch -C bf14b2f140fc84a57f61c7f59efae294b2a9a1df..a268d86e547dffe73c9f7b4633913ffdf91b1a8e
```

A new file `0001-<git-commit-first-line>.patch` is now created.

> **Note:** If you want to pick the last patch you've committed locally, you can  simplify this step by doing the following:

```shell
$ git format-patch -n
```

Where `n` is the number of last patches you want to pick (e.g. 1).

Almost there! Now send your patch to mail list using 'git send-email':
```shell
$ git send-email --to "mail@list.com" 0001-<git-commit-first-line>.patch
```