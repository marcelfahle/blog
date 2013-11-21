---
layout: post
title: "What's up with those SSH keys?"
date: 2013-09-03 10:46
comments: true
published: true
categories:
- ssh
- shell
- security
---
Alright kids, let's kick this off with some basic shell-fu, 
which will serve as a good foundation for things to come: 
**SSH Keys and how to properly use them for authentication.**

{% img right /media/images/sshkeyauth-mkay.jpg 400 225 'SSH Key authentication is good, mkay..' %}
Up until two years or so ago, I actually didn't know much about it, 
so I guess it's not so super-basic after all (or I'm just a bit behind). 
Either way, *SSH Key authentication is good, [m'kay](http://www.youtube.com/watch?v=E4_tOiLB_Ko)...* 
and it basically works like this: 

1. You create a matching key pair, a private key and a public key.
2. You add the public key to the list of authorized keys on the remote machine you want to access.
3. If then the private key on your computer matches the public key on 
the remote machine every time you try to login, you'll be granted access.

Easy as pie. And besides being more secure than the regular password authentication, it
is also a ton more convenient. So let's dive into more detail, shall we? (Note: I'm working on Mac OS X here)

Key generation
--------------
SSH Keys are a pair of cryptographic thingys that look basically like a bigass string stored in a file. 
You create your keypair with `ssh-keygen`, which gives you a ton of options to create, manage and do
all kinds of stuff for your authentication key pleasure. Just run `man ssh-keygen` to learn about all
available options and the types of keys you can create. Us being total rookies, we just use the basic stuff and
focus on RSA keys, which is probably the most common cryptography algorithm for SSH keys and definitely 
what all the cool kids use these days.

To generate your keys, open a Terminal window and cd into the .ssh directory, underneath your home directory.
```
$ cd ~/.ssh
```
If the directory for whatever reason doesn't exist, just create it.
```
$ mkdir ~/.ssh
```
Now run the magic command. I'll explain what happens below.
```
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/yourusername/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/yourusername/.ssh/id_rsa.
Your public key has been saved in /Users/yourusername/.ssh/id_rsa.pub.
The key fingerprint is:
xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx user@host
```
So, we run `ssh-keygen` with the `-t` option to set the type of keys, in this case `rsa`. For me rsa is 
also the default option, so `-t` isn't really necessary, but the command looks more impressive that way.
It then asks you where to store the key, where the default is fine, so confirm by hitting the Enter-Key. It'll
then create two files, `id_rsa` which is your private key - the one you're not supposed to give to anyone, not even your
mom - and `id_rsa.pub`, your public key. Feel free to hand this one out like flyers to your next party.

Afterwards you get asked for a passphrase, which is basically like a password to use your key, except you're not
really limited on what kind of characters you can enter. The passphrase is totally optional, but every geek in the 
world will strongly suggest that you go all nuts here and enter something different than your birthday or the name of your dog.
Whitespace, punctuation, weird characters, hell, I think even chinese characters are allowed in here. Enter it a second time
to confirm and you're done. 

Other common options here are:<br /> 
`-C comment` to add a comment to your key, usually used as description of who created the keys and when.
`-b bits` Specifies how long the key is in bits. 2048 is the default and totally cool.
`-p` Lets you change the passphrase of your private key.

Installation
------------
So how do we use our shiny new keys for authentication on a server now? 
Well, we just have to tell our server all about your new public key, which means, we have to add 
it to the list of authorized keys of the user you're planning to access the server with - we're using the root user in this example. 
To do that, we can just use one kick-ass ninja command:
```
cat id_rsa.pub | ssh root@superduperserver.com 'cat >> .ssh/authorized_keys'
```
Pure Magic! Here's what it does: the first part to the left of that vertical bar thingy (pipe) usually prints out 
whatever is in the id_rsa.pub file, but instead of printing it out into your Terminal window 
(some smartass might call it STDOUT), the vertical line
**forwards** whatever comes out of `cat id_rsa.pub` into the next command, so that it can be used there. The common man usually calls this 
"piping". 
<br/>
So now that public key gets "piped" into the following ssh command, it logs you as a root user onto your
server (this is the only time you have to enter you root password), and then executes another command - that's the stuff
in the single quotes: `'cat >> .ssh/authorized_keys'`. 
And here the `cat` "cats" our output from the first `cat` command and appends (that's what `>>` does) it
into a file called `authorized_keys`, which lives in the `.ssh` directory of the user's home directory.
`authorized_keys` holds, as the name might suggest, all the public keys that are allowed to log into 
your server, of course only regarding the user that file belongs to - in our case `root`.

In the case that you're greeted with an error message, that the `.ssh` directory doesn't exist, just hack a more advanced version 
of our ninja command into your terminal, to take care of the creation of this very directory:

```
cat id_rsa.pub | ssh root@superduperserver.com 'mkdir .ssh; cat >> .ssh/authorized_keys'
```
Either way, that's all there is to it. And now, it's time to...


Use the keys, young Skywalker
-----------------------------
That's really the easiest part of this article. You can now just log into your server with 
the standard ssh command without entering a password at all:

```
ssh root@mysuperduperserver.com
```
And that should do it! I could probably philosophize hours about why it's bad to use the root user 
for this, or how to make your server more secure, but in case I ever wanna make some serious coin with this
blog, I better write another article for that stuff. But for now, it's time to say goodbye. Comments, Facebook likes,
Retweets and whatever else helps making me rich and famous, is much appreesh! :-)

