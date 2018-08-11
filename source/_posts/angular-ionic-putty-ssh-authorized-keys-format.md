---
title: 'Angular IONIC, Putty SSH, and authorized_keys Format'
tags:
  - angular
  - ionic
  - putty
  - ssh
url: 4494.html
id: 4494
comments: false
categories:
  - IONIC
date: 2017-11-07 11:29:21
---

I started playing with IONIC last week.  Since I do all my development on a Windows computer, this has already lead to a particular Windows based challenge that I was able to track down and deduce the solution for.  But I was unable to find a solution that specifically addressed using IONIC, putty SSH, and authorized\_keys Format, which is what someone is likely to search for when they see this issue. <figure>![](/uploads/2017/10/2017-11-07.jpg "Angular IONIC, Putty SSH, and authorized_keys") Photo via [VisualHunt](//visualhunt.com/re/64a968)</figure>

<!-- more --> 

Connect to the dashboard?
-------------------------

When you setup an IONIC project, everything goes pretty much as planned until you get to the question "Do you want to connect to the dashboard?"  I'm going to assume you've answered that question in the affirmative and that you've already created an IONIC account and successfully logged in or you wouldn't be here. 

The next thing that is likely to happen is that you'll try to have it generate an SSH key for you.  But, since you don't have OpenSSL installed, it failed.  The next thing you probably did was to search for the Windows install of OpenSSL only to find that it hasn't been maintained in quite a while and that the recommended way of creating a public/private key pair is to use Putty.  Which you did. 

Now, what brought you here is that you have a public key in a file that you are trying to upload to IONIC.  The error you are continuing to see is "_filename_ does not appear to be a valid SSH public key. (Not in authorized_keys file format.)

No Public Key?
--------------

OK.  Maybe you got to this site and you don't have a public/private key pair yet.  You don't even know what putty is.  You can find some good instructions about installing and [using putty here](//www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).

You will be most interested in the section on using PuttyGen to create a public/private key pair (you want to use RSA, the default).

Putty to authorized_keys
------------------------

And the question you have now is, "How do I convert my putty SSH public key into "authorized_keys format?"  Whatever that is. 

Well you are in luck because, I can tell you exactly what you need to do. 

First, open that public key text file you have in a text editor.  You'll see it is in the format of: 

``` text
---- BEGIN SSH2 PUBLIC KEY ----
Comment: "rsa-key-20171026"
AAAAB3NzaC1yc2EAAAABJQAAAQEAicxYxlgFjQDMtkuuCY3GsnkdWEy38NSgi7N8
/SBj1ohas2GU/0bY2H6Inpg6lQ6oYmvaIy5rQZ8+pcOPQNLNo0EyWbWhmBfWI+b5
XHg5F6eIhaTjKVViJG3tDe3V4boMvgE18Rzof8ZJSa7cH0kqL32SPqnQdgtpDNc5
qjBHN+mQ5fMcxMM4YaViGtq8j6M7obt7oJrnVQWdPWQQYuo6TSm4LCjQ4/q77vqF
L59IFFd6nUnEltpkrh8RXKu6By1+G4eN66AdpZ/h9Z41qwN/oJ2kOf0qqT1t6Wa6
8RcgWxSIgMzE16imwB84o8p9DQtnEb7BVK0UXzt+Wp2ToC3arw==
---- END SSH2 PUBLIC KEY ----
```

The first thing you want to do is to delete the first two lines and the last line.  Once that is done, remove the line feeds so that the remaining RSA key is all on one line. 

``` text
AAAAB3NzaC1yc2EA...t+Wp2ToC3arw==
```

Now, prefix the line with "ssh-rsa ".  Make sure there is a space after "ssh-rsa" and before the body of the key. 

``` text
ssh-rsa AAAAB3NzaC1yc2EA...t+Wp2ToC3arw==
```

Officially, this puts the key in "authorized_keys file format" but to get IONIC to accept it, you also need to put the email you used when you signed up for IONIC dashboard at the end of the key.  This mean you'll need to add a space after the double equals that indicates the end of the key, and then your email address goes immediately after that. 

Save the file.  

Unless you like living dangerously, you should save it to another file.  

Now upload the file to ionic using: 

``` shell
ionic ssh add
```

This command will prompt you for the file name, as before, but this time you'll give it the file you just saved, and it should work.  The only reason I can think of why it might not is if you deleted too many characters when you were removing the line feeds.

IONIC Private Key
-----------------

For some reason, IONIC only understands OpenSSL for both sides.  I wasted nearly a day trying to get PAgent working.  Don't waste your time. 

Back in PuttyGen, there is a Conversions menu option.  Under that, what you want to select is the option "Export OpenSSH Key..."  Give it a filename etc. 

Now, back in the terminal window for your IONIC project, finish the setup as they've directed on their site, but just before you run "git push ionic master" run 

``` shell
ionic ssh use path_to_the_filename_you_just_exported.
```

Once you've done this, go to `c:\users\[username]\.ssh` and look for a file named "`config`".  Open the file in an editor that understands files with no carriage returns and make sure the line that starts with "IdentityFile" has quotes around the path to the file if the path has a space in it.  On my computer it doesn't, but I've seen reports from others that this has caused them hours of trouble. 

With all of that configured, now you can run 
``` shell
git push ionic master
```

If your private key has a password, it will prompt for the password and you are off to the races. 
