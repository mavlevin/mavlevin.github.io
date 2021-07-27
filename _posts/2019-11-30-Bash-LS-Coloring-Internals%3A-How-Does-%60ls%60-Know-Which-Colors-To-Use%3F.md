---
layout: post
title: >
    Bash LS Coloring Internals: How Does `ls` Know Which Colors To Use?
hide_title: false
tags: ['not security', 'C/C++']
excerpt_separator: <!--more-->
---

{% include image.html url="/assets/img/posts\ls_slash_dev.png" caption="" alt="" %}
Many of us take for granted ls's convenient display, and probably didn't ever stop to consider how it even knows which colors to use for which files. This very question sparked my curiosity and lead me to researching the internals of this mechanism.
<!--more-->

While ls is open source and you can read its [code](https://github.com/coreutils/coreutils/blob/master/src/ls.c) to understand the underlying logic, I decided not to do so as I wanted to take a black box approach.

*tl;dr at end of post*
## How Does ls Identify File Types?
### Do File Contents Matter?
I engineered two simple test to check if ls takes into account a file's content when it chooses its color:
1. I created empty files each with a different extension and ran ls to see which colors it selected for the files
2. I exchanged the contents of an image and executable and ran ls to see which colors it selected for the files

The first experiment showed that ls uses the filename's extension to select a color when the file is empty.

{% include image.html url="/assets/img/posts\ls_listing_empty_files.png" caption="Experiment #1: ls colors empty files according to their extension" alt="" %}
The second experiment further showed that ls depends on files' names rather than their contents: after the file contents in the experiment files have been switched, ls kept the same colors as it had previously used.

{% include image.html url="/assets/img/posts\replaced_files_same_color.png" caption="Experiment #2: ls colors remain unchanged even after file contents are exchanged" alt="" %}
In conclusion, we can declare that ls doesn't take the file contents into account when printing it in color in the terminal, rather ls uses the file name to decide on a color.
### What About Special Files?
ls is smart in that it can identify special files that don't have a file extension. For example, ls can identify symbolic links in the file system and color them appropriately. How does ls do this?

{% include image.html url="/assets/img/posts\ls_identifying_links.png" caption="ls identifying symbolic links and coloring them correctly" alt="" %}
#### Debugging
I ran `strace` on `ls` to see which system calls ls executes.

{% include image.html url="/assets/img/posts\ls_using_stat_syscall.png" caption="strace running on ls" alt="" %}
I noticed ls executes `stat/fstat/lstat`. These functions return in depth information about the file such as its permissions and any special properties the file may have. Bingo\! That's how ls can detect special files such as character drivers \(like those in /dev/\), executable files, and symbolic links.
### Which Property Is Given Priority In Special Files, The Filename Or File Type?
I was curious about a situation if for example you have an executable named  "image.png". Would ls color it in purple, identifying it as an image, or in bright green, identifying it as an executable?

{% include image.html url="/assets/img/posts\exe_with_image_name.png" caption="" alt="" %}
The result of the experiment shows that the filename isn't used as a classification method if the file has other special attributes such as being executable.
### How Does ls Decide Which Colors To Use For Which Files?
Opening the manual page for ls and searching for "color" brings us to a clear statement on how ls decides which colors to use for which files:

{% include image.html url="/assets/img/posts\man_page_showing_ls_colors_env_variable.png" caption="" alt="" %}
ls uses the environment variable `LS_COLORS` to select the appropriate color of a file.
### tl;dr** and recap**
ls uses the file name and information from the stat\(\) system call, such as file type, to classify the file. ls then parses the configuration in the `LS_COLORS` environment variable to select an appropriate color for the file based on its classification.
