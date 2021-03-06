# Editing P4 programs


## Color highlighting and auto-indenting

P4 is close enough in syntax to C/C++ that Emacs `c++-mode` works well
for color highlighting, and figuring out the indentation level of
code.

Force current buffer/file-being-edited to use C/C++ syntax for color
highlighting and auto-indent:

* Emacs: `M-x c++-mode RET`
* Vim: `:set syntax=c RET`

Cause all files with a suffix of `.p4` in their file name to
automatically use C/C++ mode when loaded:

* Vim: Add lines like the following in your `$HOME/.vimrc` file:

```
    augroup filetypedetect
      au BufRead,BufNewFile *.p4 setfiletype c
      " associate *.p4 with c filetype
    augroup END
```

* Emacs: Add a line like this in your `init.el` file:

```
    (setq auto-mode-alist (cons '("\\.p4$" . c++-mode) auto-mode-alist))
```


## cscope

While the [`cscope`](http://cscope.sourceforge.net/) program was
originally written for C/C++ programs, it works quite well on P4
programs, too.  To install it on Ubuntu Linux:

    sudo apt-get install cscope

The first command below creates a file `cscope.files` that contains a
list of names of all files with suffixes `.p4` and `.h` in the current
directory and all subdirectories.  The second command reads that list
of file names, and builds an index file `cscope.out` of symbols in
those files.

    find . -name '*.p4' -o -name '*.h' > cscope.files
    cscope -bk

If the links below do not help you using cscope in your chosen editor,
check the [`cscope` page](http://cscope.sourceforge.net/) for more.

* Vim: One page found by Google search for the terms "vim cscope"
  [here](http://cscope.sourceforge.net/cscope_vim_tutorial.html)
* Emacs: `xcscope` is an Emacs package that makes it straightforward
  to use the cscope search capabilities from inside of Emacs.


## Tags

[Exuberant ctags](http://ctags.sourceforge.net/) can be configured to
create tags for Emacs, Vim, and many other text editors for quickly
finding definitions of some symbols, such as actions, control flow
functions, and headers.  On Ubuntu Linux, it can be installed with:

    sudo apt-get install exuberant-ctags

To create a tags file for a collection of files containing a single P4
program, first edit your `$HOME/.ctags` to add lines like the ones in
the file [`dot-ctags-for-p4-14.txt`](dot-ctags-for-p4-14.txt) or
[`dot-ctags-for-p4-16.txt`](dot-ctags-for-p4-16.txt).

To create a TAGS file for use with Emacs, for all source files with a
`.p4` or `.h` suffix in the current directory and all sub-directories,
use this command:

    ctags-exuberant -e -R

Remove the '-e' command line option if you want to generate a 'tags'
file for Vim.

You can also give an explicit list of file names instead of the -R
option, if there are some files you do not wish to include.  The
following command achieves a similar effect as the one above, but may
be more easily modified to include or exclude the files you want:

    find . -name '*.p4' -o -name '*.h' | xargs ctags-exuberant -e

Instructions for using 'tags' file to navigate source code:

* Vim: http://vim.wikia.com/wiki/Browsing_programs_with_tags
* Emacs: https://www.emacswiki.org/emacs/EmacsTags


## Notes on P4_16 keywords

Listed below are P4_16 reserved keywords, followed by whether I think
the keyword ought to cause entries to be created in tags files for
navigating withing P4 source code using vim/emacs/etc.

First the list of things that would be nice to improve tags creation
for:

* enum - yes for enum name, but not sure how to implement tags for the values inside the enumeration.
* error - there is no name for the error declaration as a whole, but similar to enums, it would be nice to have tags for the individual error names enumerated within.
* extern - yes, done, at least for the extern name as a whole.  How to do it for functions declared within the extern, though?
* match_kind - no name for the match_kind declaration as a whole, but similar to enums and errors, it would be nice to have tags for the individual match_kind names enumerated within.
* tuple - would be nice, but not sure how to specify syntax with exuberant ctags accurately, since what can follow keyword 'tuple' before the name of the tuple defined can have nested angle brackets that should balance.


Now the list of things that I believe are fairly accurately done
already:

* action - yes, done
* apply
* bit - no
* bool - no
* const - yes, done (not sure if I have all characters that can appear in type name)
* control - yes, done
* default - no
* else - no
* exit - no
* false - no
* header - yes, done
* if - no
* in - no
* inout - no
* int - no
* out - no
* package - yes, done
* parser - yes, done
* return - no
* select - no
* state - yes, done
* struct - yes, done
* switch - no
* table - yes, done
* transition - no
* true - no
* typedef - yes, although syntax parsing of type name might not be 100% accurate.
* varbit - no
* verify - no
* void - no
