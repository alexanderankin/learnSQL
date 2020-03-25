---
layout: page
permalink: /downloads/
title: Downloads
---
# Downloads

In order to set this up fully in the "cloud", you will need:

1. [Git BASH][gitbash] for connecting (not needed for Windows/Mac)
1. [PHPMyAdmin][pma], the program which is the main subject

## Local Installations

If you wish to follow along without a rented server, you will not be able to
collaborate, ensure reliability, but it will save you 5 bucks every month. To
install PHPMyAdmin locally, on windows you'll also need [Apache XAMPP][xampp].

## Git BASH

> Better than `cmd` and has `ssh` built-in.

_git_ - [the stupid content tracker][sct] is a "fast, scalable, distributed
revision control system with an unusually rich command set". The reason for
this requirement is for the BASH shell.

_BASH_ stands for Bourne-Again Shell (no not the religious sense). On a linux
computer, this is usually the program that will process commands and other
keyboard input in a terminal emulator. Most programs developed for this use
expect lots of auxiliary linux programs to be present, like they are here.
More on [BaSH][bash], and on [shells][shell].

In particular, the command `ssh` will be present, which will we will use to
connect to "the cloud" to install PHPMyAdmin. It stands for Secure Shell, as
in securely connecting you to a different (remote) shell.

### Installation Options

Here are the screens you will see when installing, the questions on them, and
my recommended answers.

|Screen|Question|Answer|
|-|-|-|
|Information|Do you accept the GPL License|Click Next|
|Select Destination Location|Where to install|Click Next|
|Select Components|Addt'l Icons|Personal Preference|
|Select Components|Addt'l Icons: On the Desktop|Personal Preference|
|Select Components|Win. Exp. Integration||
|Select Components|Win. Exp. Integration: Git Bash Here|**Checked**|
|Select Components|Win. Exp. Integration: Git Here|Unchecked|
|Select Components|Git LFS (large file storage)|**Checked**|
|Select Components|Assoc. `.git` with default editor|Unchecked|
|Select Components|Assoc. `.sh` to run w/ Bash|**Checked**|
|Select Components|Use a TrueType Font|Unchecked|
|Select Components|Check Daily|Unchecked|
|Select Start Menu Folder|Folder Name|Unchecked|
|Select Start Menu Folder|Don't create a folder|**Checked**|
|Adjusting your PATH environment|(pick one)|Use Git Bash Only|
|Choosing HTTPS|(pick one)|Use the OpenSSL Library|
|Configuring line ending cv's|(pick one)|Checkout Windows, Commit UNIX|
|Configuring terminal emulator|(pick one)|Use MinTTY|
|Configuring extra options|Enable file system caching|**Checked**|
|Configuring extra options|Enable Git Credential Mapper|**Checked**|
|Configuring extra options|Enable Symbolic links|Unchecked|

I used [this site][gitbashscreens] for reference to what the options are.


## PHP My Admin

This is a Web Application (runs on a server, used from a browser) to browse,
configure, edit a [MySQL][mysql] Database. [Some][mysql1] [MySQL][mysql2]
[basics][mysql3].

[gitbash]: https://gitforwindows.org/
[pma]: https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.zip
[xampp]: https://www.apachefriends.org/download.html
[sct]: https://git-scm.com/docs/git
[bash]: https://www.gnu.org/software/bash/manual/bash.html#What-is-Bash_003f
[shell]: https://www.gnu.org/software/bash/manual/bash.html#What-is-a-shell_003f
[mysql]: https://www.mysql.com/products/community/

[mysql1]: https://www.mysqltutorial.org/basic-mysql-tutorial.aspx
[mysql2]: https://www.w3schools.com/sql/sql_intro.asp
[mysql3]: https://www.guru99.com/how-to-create-a-database.html

[gitbashscreens]: https://www.siteground.com/tutorials/git/windows-installation/