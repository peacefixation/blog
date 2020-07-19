---
title: "Add an offset to a list of numbers with Vim"
date: 2017-08-30 21:00:00 +1000 
draft: false
toc: false
images:
categories:
  - development
tags: 
  - development
  - vim
---

Today I was given a task to extract customer account IDs from a log file. `grep` came to the rescue, however I was then required to add a fixed offset to each account ID so I could build an SQL query with an `IN` clause. I could have managed this by piping the output of `grep` to `awk` or `bc`, but I thought I would flex my `vim` muscle in this case.

Given a file containing a list of numbers on consecutive lines, I want to add an offset of 10 to every number.

{{< highlight sql >}}
1
2
3
4
5
6
7
8
9
{{< / highlight >}}

First a quick Vim refresher. Vim has modes. Normal mode is for navigation and text manipulation, everything you type is interpreted as a command. You can use commands like `gg` to go the start of the file, `G` to go to the end of the file, `$` to go to the end of the line, etc. You can return to normal mode from another mode by pressing `ESC`. Command-line mode is for Vim commands. You enter command-line mode by pressing `:`, then you enter your command. For example `:set nu` turns line numbering on. `:help  command-line-mode` opens the help file for command-line-mode. With that out of the way, we are going to record a macro and then execute it by running a normal mode command from command-line mode with the `normal` command. Vim is complicated!

Create a file with the numbers 1-9 on consequtive lines. Press `ESC` to make sure we're in normal mode. Now we will record a macro called `a`:

- type `qa` to start recording a macro called `a`
- type `10` and press `Ctrl-A` to increment the first number by 10
- type `q` to stop recording

You can use `:u` to undo the last command to revert that change, alternatively type `10` and press `Ctrl-X` to decrement the number by 10.

Now we will apply the macro to every line in the file, from normal mode:

- type `:%normal! @a`

Let's dissect that command. 

The `:` enters command mode. `%` means select the entire range from the start of the buffer to the end. `normal` means execute a normal mode command, but that's not enough. `normal!` means execute a normal mode command and ignore any remappings. If the user has remapped the `@` command to something else we will ignore that mapping. `@a` executes the macro named `a`.

And there it is, an offset added to every number!

{{< highlight sql >}}
11
12
13
14
15
16
17
18
19
{{< / highlight >}}

I had some help from a Stack Overflow [answer](https://stackoverflow.com/questions/390174/in-vim-how-do-i-apply-a-macro-to-a-set-of-lines), and  I also found a nice [Vim tutorial](http://learnvimscriptthehardway.stevelosh.com/) where I learned about the `normal!` command.

Next I had to pull the email addresses out of the database. I included my account numbers in an SQL query with a little help from Vim's substitute command.

- `:%s/$/,/`

What's this doing? Well `:` enters command mode and `%` selects the whole buffer. Then `s` is the substitute command and we find the end of each line with `$` and replace it with a `,`. So now we have a comma after each number, and it's easy to go from this:

{{< highlight sql >}}
11,
12,
13,
14,
15,
16,
17,
18,
19,
{{< / highlight >}}

to this (which I did manually):

{{< highlight sql >}}
SELECT email_address 
FROM the_table
WHERE account_number IN (
11,
12,
13,
14,
15,
16,
17,
18,
19)
{{< / highlight >}}

And there you have it; a tedious, everyday task made quick and easy with the help of an advanced text editor, and one more trick in my box of Vim tricks. 
