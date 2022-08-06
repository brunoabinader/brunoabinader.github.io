---
title: "Vim Quicktip: Navigating to function implementation with ctags"
date: 2012-04-12
---

I am a regular [*Vim*](https://www.vim.org/) user, but I've also played around with full blown IDEs like [*Qt Creator*](https://www.qt.io/product/development-tools) and [*Visual Studio*](https://visualstudio.microsoft.com/). One interesting aspect about IDEs is the ability to easily navigate through a function implementation based on its usage on a piece of code. Taking the example
from my [CMake
post](/2009/12/07/how-cmake-simplifies-the-build-process-part-1-basic-build-system/),
suppose you use the function *sayHello()* defined in a included header
of your current source file and its implementation resides in another
file as well. You may want to check out its implementation, but you
doesn't actually require to open a new view or loading it on the
current view without an easy way to get back to the previous code. For
this, Vim works very well with ctags. Let us check out how to generate
ctags for your code and use it to let Vim know where to look up for your
code.

First of all, we are assuming you already have Vim installed on your
system, so now we must also have the *exuberant-tags* package as well:

```shell
$ sudo apt-get install exuberant-ctags
```

Now that you have ctags package installed, you can start generating
ctags among your commonly used source code projects. You can generate a
*ctags* file manually by issuing the following command (preferably
inside your project's base directory):

```shell
$ ctags -R --sort=yes .
```

It is wise to keep an up-to-date *ctags* file for your most used source
code projects, and then tell Vim to load them every time you open a file.
For this, you can add the following line to your `~/.vimrc` file:

```vim
set tags += ~/.vim/tags/cpp " (standard C++ library)
```

> **Note:** You can also create a macro that automagically creates a ctags
file on the directory where your currently open file on Vim is located.
For this, you can also add this line of code inside `~/.vimrc`:

```vim
map <C-F12> :!ctags -R --sort=yes .<CR>
```

Now every time you press *Ctrl+F12* inside Vim, the *ctags* file is
either created or updated if already exists. You are then able to navigate through your code implementation. Try putting your cursor on a given function call, then press `Ctrl+]` to go to its implementation, and `Ctrl+t` to get back to where it has been called. You can also manually navigate to a function implementation by requesting from Vim:

```vim
:ta <function_name>
```