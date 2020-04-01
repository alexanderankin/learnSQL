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

#### 1. Signup for DO [`next`](#2-do-cloud-management-next)

[do-pricing]: https://www.digitalocean.com/pricing/#Compute

[//]: # (image, src below)
![Digital Ocean Signup Page][do-signup2]

[do-signup2]: {{ '/img/do_signup2.jpg' | relative_url }} "Digital Ocean Signup Page"

#### 2. DO Cloud Management [`next`](#3-create-droplet-next)

Once done, go to the [Cloud Managment][do-cloud] page which looks like this:

[do-cloud]: https://cloud.digitalocean.com/projects

[//]: # (image, src below)
![Digital Ocean Project Manager][do-prj-mgr]

[do-prj-mgr]: {{ '/img/do_cloud.jpg' | relative_url }} "Digital Ocean Project Manager"

#### 3. Create Droplet [`next`](#4-create-ssh-key-next)

From there click create, then droplet. A Droplet is their branded word for a
virtual, server, a computer you can log onto and use like you would a personal
computer. As per usual, you want to select all the leftmost options. You can
[skip to summary](#summary) if you just want my condensed opinions.

[//]: # (image, src below)
![Create A Droplet][do-create]

The first setting you see is "Image" or Operating System. I recommend choosing
Ubuntu 19: getting support will be easier. Make sure to **CLICK THE LEFT ARROW**
to scroll all the way on all the options, especially when selecting droplet
size, you'll want to start with the basic size, not the one that is `$40` /
month.

[//]: # (image, src below)
![Select the right size][do-ubuntu19]
![Select the right size][do-price-hidden]


[do-create]: {{ '/img/do_create.jpg' | relative_url }} "Click Create then Droplet"
[do-ubuntu19]: {{ '/img/do_ubuntu19.jpg' | relative_url }} "Ubuntu 19"
[do-price-hidden]: {{ '/img/do_price_hidden.jpg' | relative_url }} "Select the right size, $ 5 / month"

#### 4. Create SSH Key [`next`](#5-connect-to-ssh-next)

In order to connect securely, you'll need to generate an RSA SSH key and paste
it into the form to create a droplet.

Digital ocean actually provides good instructions in the interface, with links
to articles in case of confusion, but I will cover this here for completeness.
On windows you should either follow [their guide][do-ssh-keys] or, in Git
BASH, do as for linux. On linux:

```
cd                  # go to your home folder
ssh-keygen -t rsa   # make rsa-type key, hit enter to accept default location
cat .ssh/id_rsa.pub # print public key from default location
```

> Make sure you have enabled [Copying/Pasting shortcuts][setup-copy-paste].

Then, copy and paste **the whole output** into the form. This piece of
information is actually not secret. Using this data, someone can send you a
message only you can read. When you connect to your server with it, no one can
see your screen if they intercept it, or see the output if they connect
pretending to be you. Not even the government.

![i][do-ssh-key-form]

[do-ssh-key-form]: {{ '/img/do_ssh_key_form.jpg' | relative_url }} "Whole key in form"

[do-ssh-keys]: https://www.digitalocean.com/docs/droplets/how-to/add-ssh-keys/create-with-putty/
[setup-copy-paste]: {{ '/downloads#git-bash-copy-paste' | relative_url }}

#### 5. Connect to SSH [`next`](#6-install-stuff-on-the-droplet)

Once you submit those options, you can go to your [droplets][do-droplets] and
get the IP Address for your newly created droplet. after you have it, you can
connect like so: 

```
$ ssh root@11.222.3.444
The authenticity of host '11.222.3.444 (11.222.3.444)' can't be established.
ECDSA key fingerprint is SHA256:-----REDACTED--------.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '11.222.3.444' (ECDSA) to the list of known hosts.
Welcome to Ubuntu -----REDACTED-----
....
```

#### 6. Install Stuff On The Droplet [`next`](#summary)

> In summary, make a non root user, install php, nginx, mysql, unzip and chown
> the latest phpmyadmin.

Try to pick the user as your local computers username, that way you will not
have to type it in every time you connect.

* Creating a non-root user (for security) by running `adduser sqllearner`

```
root@learnSQLDemo:~# adduser sqllearner
Adding user `sqllearner' ...
Adding new group `sqllearner' (1000) ...
Adding new user `sqllearner' (1000) with group `sqllearner' ...
Creating home directory `/home/sqllearner' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for sqllearner
Enter the new value, or press ENTER for the default
  Full Name []: 
  Room Number []: 
  Work Phone []: 
  Home Phone []: 
  Other []: 
Is the information correct? [Y/n] Y
```

* Adding your login details to that user

here we are copying the `authorized_keys`, *mod*ifying to make them readable
(`644`). We are *s*ubstituting the *u*ser (`su`) as the new user. Then we are
going to our home folder (`cd`), and making the same folder, with the same
file, and changing permissions back to everything for us (`7`) and nothing for
other's permissions (`00`). The `-R` does this to the whole folder. Then we
exit that user back to the `root` user, and clean up the file we used. We then
make the user an administrator and logging out.

```
root@learnSQLDemo:~# cp .ssh/authorized_keys /authorized_keys && chmod 644 /authorized_keys 
root@learnSQLDemo:~# su sqllearner
sqllearner@learnSQLDemo:/root$ cd
sqllearner@learnSQLDemo:~$ mkdir .ssh && cp /authorized_keys .ssh/ && chmod 700 .ssh -R
sqllearner@learnSQLDemo:/root$ exit
exit
root@learnSQLDemo:~# rm /authorized_keys
root@learnSQLDemo:~# usermod -aG sudo sqllearner
root@learnSQLDemo:~# exit
logout
Connection to 11.222.3.444 closed.
```

* Login as that new user, and verify that we are an administrator (aka
`sudoer`).

If your forgot your password, you can always log back in as root and run
`passwd sqllearner`.

```
$ ssh sqllearner@11.222.3.444
...
...
...
sqllearner@learnSQLDemo:~$ sudo su
[sudo] password for sqllearner: 
root@learnSQLDemo:/home/sqllearner# exit
sqllearner@learnSQLDemo:~$
```

* Install the programs available as packages, giving it `y` or `yes` where
necessary.

```
sudo apt-get update
sudo add-apt-repository ppa:ondrej/php
sudo apt-get install -y php7.4-fpm php7.4-mysql php7.4-mbstring nginx mysql-server unzip
```

* Set the root password for MySQL Server by running `sudo mysql` and running
these commands:

```
mysql> alter user root@localhost identified with caching_sha2_password by 'mynewpwd';
Query OK, 0 rows affected (0.01 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> \q
Bye
```

* Configure `PHPMyAdmin`

So, what we came here to do in the first place.

```
sqllearner@learnSQLDemo:~$ wget "https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.zip"
--2020-04-01 10:32:34--  https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.zip
Resolving files.phpmyadmin.net (files.phpmyadmin.net)... 195.181.169.26
Connecting to files.phpmyadmin.net (files.phpmyadmin.net)|195.181.169.26|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14199213 (14M) [application/zip]
Saving to: ‘phpMyAdmin-5.0.2-all-languages.zip’

phpMyAdmin-5.0.2-al 100%[===================>]  13.54M  --.-KB/s    in 0.07s   

2020-04-01 10:32:34 (184 MB/s) - ‘phpMyAdmin-5.0.2-all-languages.zip’ saved [14199213/14199213]

sqllearner@learnSQLDemo:~$ unzip -qq phpMyAdmin-5.0.2-all-languages.zip 
sqllearner@learnSQLDemo:~$ cd phpMyAdmin-5.0.2-all-languages/
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$ cp config.sample.inc.php config.inc.php 
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$ # we are uncommenting some lines, pma settings
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$ sed -i '/pmadb/{s/^\(...\)\(.*\)$/\2/g}' config.inc.php
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$ sed -i '/pma__/{s/^\(...\)\(.*\)$/\2/g}' config.inc.php
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$ # generate a security token
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$ SECRET=$(echo 'blahblah' | md5sum | awk '{ print $1 }')
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$ # put in the quotes in the line with "blowfish"
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$ sed -i "/blowfish/{s/''/'$SECRET'/}" config.inc.php
```

* Make `PHPMyAdmin` available over the internet.

```
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$ dir=`pwd`
sqllearner@learnSQLDemo:~/phpMyAdmin-5.0.2-all-languages$ cd
sqllearner@learnSQLDemo:~$ sudo chown -f www-data:www-data $dir -R
sqllearner@learnSQLDemo:~$ sudo ln -s $dir /var/www/html/customizeYourPMAUrl
```

* Make `PHP FPM` (PHP Fast Process Manager) available to `nginx`.

```
sqllearner@learnSQLDemo:~$ # filter out comments and save to default in current folder
sqllearner@learnSQLDemo:~$ # filter out anything that is some white space and a pound symbol, minus last 3 lines
sqllearner@learnSQLDemo:~$ cat /etc/nginx/sites-available/default | sed '/^[[:space:]]*#/d' | head -n -3 > default
sqllearner@learnSQLDemo:~$
sqllearner@learnSQLDemo:~$ # append php magic to configuration
sqllearner@learnSQLDemo:~$ echo 'location ~ \.php$ { include snippets/fastcgi-php.conf; fastcgi_pass unix:/var/run/php/php7.4-fpm.sock; } }' >> default
sqllearner@learnSQLDemo:~$
sqllearner@learnSQLDemo:~$ # copy it back
sqllearner@learnSQLDemo:~$ sudo mv default /etc/nginx/sites-available/default
sqllearner@learnSQLDemo:~$
sqllearner@learnSQLDemo:~$ # add index.php because i forgot it earlier
sqllearner@learnSQLDemo:~$ sudo sed -i '/index/{s/nginx-debian.html/php/}' /etc/nginx/sites-available/default
sqllearner@learnSQLDemo:~$
sqllearner@learnSQLDemo:~$ # verify it looks like this
sqllearner@learnSQLDemo:~$ cat /etc/nginx/sites-available/default 

server {
  listen 80 default_server;
  listen [::]:80 default_server;


  root /var/www/html;

  index index.html index.htm index.php;

  server_name _;

  location / {
    try_files $uri $uri/ =404;
  }


location ~ \.php$ { include snippets/fastcgi-php.conf; fastcgi_pass unix:/var/run/php/php7.4-fpm.sock; } }
sqllearner@learnSQLDemo:~$
sqllearner@learnSQLDemo:~$ # test it for correctness
sqllearner@learnSQLDemo:~$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
sqllearner@learnSQLDemo:~$
sqllearner@learnSQLDemo:~$ # reload the nginx configuration
sqllearner@learnSQLDemo:~$ sudo service nginx reload
```

* Give sudoers permission to edit, in case previous steps need to be fixed.

First we go into the folder, then we refer to it as the dot (`.`). The `chown`
syntax means do not *ch*ange *own*ership as user, which usually precedes the
optional colon, but the group. The `chmod` command means *ch*ange the *mod*e
of these files to be `+sw`, sticky and writable, for groups. That is, when new
files appear, they will have the same group modes. both commands are run
recursively, with `-R`.

```
cd phpMyAdmin-5.0.2-all-languages
sudo chown :sudo . -R
sudo chmod g+sw . -R
```

#### Done!

Now you can go to your portal by opening a browser and going to the url:
`http://.../customizeYourPMAUrl` where the elippsis are replaced by the ip
address of your droplet, which looks like `11.222.3.444`. You can log on with
the default username `root` and the password which you set earlier, I used
`mynewpwd`.

The first thing you need to do is scroll to the error at the bottom, click
`Find Out Why`, and then click on the link to create the database to store
your settings for PHPMyAdmin.

> Fin!

[do-droplets]: https://cloud.digitalocean.com/droplets

#### Summary

Here's the short version of what to put.

|Heading|Choice|Explanation|
|-|-|-|
|Choose an image|Ubuntu 19|Choose an operating system that its easy to google about|
|Choose a plan|5/mo|For a demo choose smallest, SQL also doesnt work *that* responsively with bigger size|
|Add block storage|Skip|For the record, this is how you should add size, not with a big plan|
|Choose a datacenter region|NY|Or the one closest to you|
|Select additional options|Skip|These take time to research fully, not necessary for demo|
|Authentication|SSH Keys|Setup a key, if you don't you **will** get hacked|
|Finalize and create|`1`|Name is not important|
|Add tags|Skip|This is for purely organization purposes|
|Select Project|Skip|This is for purely organization purposes|
|Add Backups|Skip|Not necessary for demo|


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
