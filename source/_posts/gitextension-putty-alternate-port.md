---
title: 'GitExtension, Putty, Alternate Port'
tags:
  - Git
  - gitextensions
  - gitosis
  - putty
  - ssh
url: 1844.html
id: 1844
categories:
  - Git
date: 2010-04-26 06:01:00
---

![image](/uploads/2010/04/image8.png "image")Last week I showed you how to [install GitExtensions to access Gitosis](/2010/04/22/git-gitosis-putty-and-windows/) and promised that I’d show you how to get this all working when Gitosis is running SSH on an alternate port.

There are several reasons why you might want to run SSH on an alternate port.  In my case it is because my ISP blocks incoming traffic on the lower ports and I want to be able to access my computers using SSH’s tunneling feature when I’m on the road.

The trick lies in a feature of Putty.  If you want to do this, you’ll need to [install the Putty.exe](//www.chiark.greenend.org.uk/~sgtatham/putty/download.html) 

Actually, “install” is probably not the right word.  All you really need to do is download the putty.exe and put it in the GitExtension directory in your program files directory.

What we are going to do now is create a session to connect to our Gitosis server.  So fire up the putty.exe.

![image](/uploads/2010/04/image9.png "image")

Type in the host name or the IP address to the SSH/Gitosis server as well as the alternate port number you need to connect to.  Also, now would be a good time to give the configuration a “session name”

![image](/uploads/2010/04/image10.png "image")

The advantage of using a session like this is that we can also specify the login name and the private key file here, rather than in GitExtensions.  You can do both if you want, as long as they match.

![image](/uploads/2010/04/image11.png "image")

###### Note:  The Auto-login username you put in here will be the username of the account that is running Gitosis, NOT the account associated with your public/private keys.

![image](/uploads/2010/04/image12.png "image")

Navigate back to the main screen and save the session.

![image](/uploads/2010/04/image13.png "image")

You’ll see we saved the session as “Gitosis.”

You should test this to make sure you can connect.  What will happen is that it will briefly “connect” but then you’ll get kicked out because the server is not allowing this type of connection.  This isn’t an issue because we won’t be using putty to connect.

Now in GitExtensions when you specify the Git URL, instead of specifying the server name between the “@” and the “:”, you’ll specify the name of the session you just created:

Administrator@**Gitosis**:repositoryName.git

When you do that, the session named Gitosis will be used to make the connection instead of looking for a server named Gitosis.
