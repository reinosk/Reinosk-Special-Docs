---
title: Git commit message 基本规范
date: 2019-02-12 04:14:54
tags: [Git]
---

When using git to commit a version, the commit message is very important, as you need to be able to look back at the commit log and see what was changed each time.

When we write this commit message, we should follow a certain structure to help us standardize and clarify our thinking.

We generally follow the [**thoughtbot specification**](https://github.com/thoughtbot/dotfiles/blob/master/gitmessage), and here are their instructions [5 Useful Tips For A Better Commit Message](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message).

```bash
50-character subject line
72-character wrapped longer description. This should answer.
* Why was this change necessary?
* Why was this change necessary? * How does it address the problem?
* How does it address the problem? * Are there any side effects?
Include a link to the ticket, if any.
```

Simply put:

1. the first line should be no more than 50 characters. 2.
2. the second line is a blank line
3. the third line begins with a descriptive message, each line should be no more than 72 characters in length, with a serial number and no period at the end
4. the descriptive information starting on the third line is the main description:
   - What changes were made to this submission?
   - What changes have been made to this submission? How is the problem being solved?
   - How does it address the problem? Will it affect anything?
5. After the descriptive message, either leave a blank line and close the issue or give a link to the appropriate ticket.

Example:

```bash
fix($compile): couple of unit tests for IE9

1. Older IEs serialize html uppercased, but IE9 does not...
2. would be better to expect case insensitive, unfortunately jasmine does
3. not allow to user regexps for throw expectations

Closes #392
Breaks foo.bar api, foo.baz should be used instead
```