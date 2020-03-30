---
layout: page
permalink: /steps
title: Setup
---

These are all the downloads you will need to follow along at home. If you are
reading this page I am assuming you have a terminal emulator program (aka
command prompt) where the command `ssh` is available. [Git BASH][gitbash] is
is such a program that I describe [here][downloads].

[gitbash]: https://gitforwindows.org/
[downloads]: {{ '/downloads' | relative_url }}

## Setup a Web Server

I will assume you want to do this the modern way, on [Digital Ocean][do]'s
offerrings. However, I provide instructions to configure a Web Server locally
as well, in the [second subsection](#setup-local-server), because after you
satisfy the captcha and confirm your email, it *will* ask you for your payment
details because it renting server space costs coin.

[do]: https://www.digitalocean.com/

### Digital Ocean

After you've set up your account, you'll need to enter your payment details.
For [reference][do-pricing], this will cost you .7¢ / hr, or if you leave it
there, 5 bucks a month.

![Digital Ocean Signup Page][do-signup2]

Once done, go to the [Cloud Managment][do-cloud] page which looks like this:

![Digital Ocean Project Manager][do-prj-mgr]


[do-signup2]: {{ '/img/do_signup2.jpg' | relative_url }} "Digital Ocean Signup Page"
[do-pricing]: https://www.digitalocean.com/pricing/#Compute
[do-cloud]: https://cloud.digitalocean.com/projects
[do-prj-mgr]: {{ '/img/do_cloud.jpg' | relative_url }} "Digital Ocean Project Manager"



### Setup Local Server

#### Windows

Navigate to [the Node.js Download page][nodejs] download page, click on the
top left option:

![Node.js Windows Installer Location][nodejs-win]

Select all the default options which are described [here][nodejswintutorial]. 

[nodejs-win]: {{ '/img/nodejs_win_msi.jpg' | relative_url }} "Node.js Windows Installer Location"
[nodejswintutorial]: https://www.shaileshjha.com/how-to-install-nodejs-and-npm-on-windows-10/

#### *nix (Linux, Mac/Unix)

First setup [Node.js][nodejs] with two commands, taken from [here][debnodesrc]:

```
# Using Ubuntu
curl -sL https://deb.nodesource.com/setup_13.x | sudo -E bash -
sudo apt-get install -y nodejs
```

> These commands do the following. First `curl` is invoked with two options,
> `s` and `L`, the dash is a notation to indicate "options" instead of
> "arguments". roughly, arguments are what to do, options/flags are how. here,
> curl is run silently (`s`) and to follow redirects, when downloading a file,
> you can be redirected, like a webpage. The output of the download is send,
> using the pipe character (`|`) into `sudo` which gives admin access. `sudo`
> is run with `-E` meaning to keep the environment (information about where
> other commands exist). [`sudo`][sudo] needs to know what to do as an
> administrator, hence immediately after is `bash -`. The dash, when used as
> an argument, usually means try to find the input from what is being `pipe`'d
> in. The 2nd command is the standard way to install programs.



[nodejs]: http://nodejs.org/en/download
[debnodesrc]: https://github.com/nodesource/distributions#installation-instructions

[sudo]: https://xkcd.com/149/
