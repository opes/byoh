BYOH
====

![BYOH logo](logo.png)

### BYOH: Build Your Own Heroku
_A Getting Started Guide for Complete Noobs... like me_

--


#### <a name="top"></a>Table of Contents
1. [What is this about?](#about)
- [Who is this for?](#for)
- [Why should I even?](#even)
- [What's involved?](#involved)
- Server Setup Part 1: Creating you server
	- Prerequisites
		- [Domain Name](#domain)
		- [Git](#git)
			- [What is Git?](#what-is-git)
			- [Why should I use it?](#why-git)
			- [How do I install Git?](#install-git)
		- [SSH](#ssh)
			- [What is SSH?](#what-is-ssh)
			- [Generating SSH Keys](#generate-ssh)
	- [Setting up a DigitalOcean Droplet](#digitalocean)
		- [Sign Up](#digitalocean-sign-up)
		- [Adding Your SSH Key](#digitalocean-add-ssh)
		- [Set Up Droplet](#digitalocean-set-up-droplet)
			- Droplet options
				- [Private Networking](#digitalocean-private-networking)
				- [User Data](#digitalocean-user-data)
				- [IPv6](#digitalocean-ipv6)
				- [Backups & Snapshots](#digitalocean-backups-snapshots)
			- [Choosing an Image](#digitalocean-image)
			- [Select SSH Key](#digitalocean-select-ssh-key)
			- [Launching the Server](#digitalocean-launch)
		- Set DNS
			- [Add your domain](#dns-add-your-domain)
- Server Setup Part 2: Installing Tools & Adjusting Settings
	- [Connecting via SSH](#connect-via-ssh)
	- [Installing Dokku-alt](#installing-dokku-alt)
	- [Setting up Virtual Memory](#setting-up-virtual-memory)
		- [Bring in the SWAP](#bring-in-the-swap)
	- [Using dokku-alt-manager (dam) to create your apps and databases](#using-dam)
- [Deploying Code](#deploying-code)
	- [Initial git setup & first deploy](#initial-git-setup)
	- [Making changes and pushing them to the interweb](#ch-ch-ch-ch-changes)
- [Conclusion](#the-end)

--

# <a name="about"></a>What is this about?

Everything is online, from retail stores and shops to streaming video and social networks. This guide is intended to help readers understand how a website or web app can be put online and made available for others to access.

<small>[&#65514; Back to Top](#top)</small>

# <a name="for"></a>Who is this for?

The target audience for this guide is intentially broad.  It gives you enough overview for those that are just curious about the process, as well as granular details into how to do it yourself.

<small>[&#65514; Back to Top](#top)</small>

# <a name="even"></a>Why should I even?

Sure, there are many different ways of getting your website put onto the internets - some even easier than this.  But traditional web hosts can be problematic and a standard web hosting solution isn't usually a valid representation of how companies are getting their sites & apps online.

Many web hosts still require you to manually upload all your code & changes to their servers, but what happens if you accidentally upload the wrong file or delete something you didn't mean to?  With BYOH, readers are shown ways to prevent that from happening or even undoing something that was done accidentally.

<small>[&#65514; Back to Top](#top)</small>

# <a name="involved"></a>What's involved?

This guide will show you how to set up a server in the cloud (via DigitalOcean) to host your website or app so that others can access it on the internet.

<small>[&#65514; Back to Top](#top)</small>

--

# Server Setup Part 1: Creating you server
## Prerequisites
### <a name="domain"></a>Domain Name

A [domain name](http://netforbeginners.about.com/od/d/f/domain_name.htm) is what you type into your [browser](http://www.gcflearnfree.org/internet101/4)'s address bar to go to a website, such as _[www.google.com](http://www.google.com)_, _[whitehouse.gov](http://whitehouse.gov)_, or _[theroadhome.org](theroadhome.org)_.  Behind the scenes, the domain actually takes you to an [IP Address](http://computer.howstuffworks.com/internet/basics/question549.htm) where your server is located.  It makes it easier for others to access your website.  Can you imagine if we had to know every IP address for all of the websites we've visited?  Yikes.

Getting your very own domain name is easy and fairly inexpensive.  All that's required is [buying your domain name](https://www.namecheap.com/), then pointing it to your server (which will be covered below).

<small>[&#65514; Back to Top](#top)</small>

### <a name="git"></a>Git

_Or "how to undo that terrible change you just made that broke everything"_

<small>[&#65514; Back to Top](#top)</small>

#### <a name="what-is-git"></a>What is Git?

[Git](http://www.makeuseof.com/tag/git-version-control-youre-developer/) is a way to keep a history of changes that have been made to your code.  It allows for multiple developers to work on the same code simultaneously without one person overwriting work that another had already done.  It makes it easier to resolve any conflicts in the code before uploading it to the server to be put online.

Git (or some other version control software) is critical when working on code with more than just one person.  However, even if you are the only one writing code, it is still important to use in case you need to undo a change you made or figure out at what point in history that a piece of code was written.

<small>[&#65514; Back to Top](#top)</small>

#### <a name="why-git"></a>Why should I use it?

Imagine you are working on changing the homepage to your website.  You create a new [index.html](http://webdesign.about.com/od/beginningtutorials/f/index_html.htm) file, fill it with some junk content, and maybe even throw in a couple irrelevant pictures as placeholders.  Then you decide to scrap it, but you don't have the previous version of the file.  So you [connect to your server](https://help.yahoo.com/kb/yahoo-web-hosting/ftp-sln20433.html) to download the current version and overwrite your "work in progress".  You click the download button, click "overwrite", and get the file.  Or so you thought...  When you open up the "downloaded" file, you realize it's the same file you were working on before!  You suddenly realize that you accidentally clicked the _Upload_ button instead of _Download_.  Now, the whole world can see your junky work in progress!  And you don't have the original file anywhere to go back!  Time to call tech support...  Hope you have a couple hours to spare!

Now, what if you had used Git?  If you wanted to make the same mistake, you would have to 1.) add the file, 2.) commit your change, and 3.) push it to the server.  Hopefully you would have caught your mistake before you pushed it out.  But what if you didn't?  With Git, you have an easy way to undo that change you made, no tech support required!  You can revert your change and push it out and everything will be back to where it was before, when it was working!

Or what if you had multiple people working on the same file at the same time?  One person is making numerous, in-depth changes, where the other just had to fix a typo.  The bigger change goes out first, but then the other typo fix overwrites it (using the old version).  The developer making the big changes sees that a new version was uploaded, so they download it and replace their current version... Hopefully they had a backup!

<small>[&#65514; Back to Top](#top)</small>

#### <a name="install-git"></a>How do I install Git?

Each platform makes it fairly easy to install Git.  If you haven't installed it already, read more [here](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

<small>[&#65514; Back to Top](#top)</small>

### <a name="ssh"></a>SSH

_Or "hold onto your butts"_

<small>[&#65514; Back to Top](#top)</small>

#### <a name="what-is-ssh"></a>What is SSH?

[SSH](http://searchsecurity.techtarget.com/definition/Secure-Shell) is a way to securely connect to your web server and make changes.  But be careful!  Don't run any command if you aren't 100% sure what it does.  You could delete something you don't mean to or install something that could break or be unsafe for your server.

You use SSH through your computer's [terminal/command line application](http://blog.teamtreehouse.com/introduction-to-the-mac-os-x-command-line).  However, to connect to your server via SSH, you must set up [SSH Keys](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets) if you haven't already (see below).

<small>[&#65514; Back to Top](#top)</small>

#### <a name="generate-ssh"></a>Generating SSH Keys

If you already have SSH keys created and want to use them, you can skip this step.  However, if you don't have any or are unsure, continue on.

[How to Set Up SSH Keys on DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets)

<small>[&#65514; Back to Top](#top)</small>

## <a name="digitalocean"></a>Setting up a DigitalOcean Droplet

A Droplet is what DigitalOcean calls their virtual servers where your code will be hosted.  It's the "cloud" where people will visit your website.

Read More: [Technical FAQ](https://www.digitalocean.com/help/technical/general/)

<small>[&#65514; Back to Top](#top)</small>

### <a name="digitalocean-sign-up"></a>Sign Up

First, [create an account on DigitalOcean](http://digitalocean.com), add your billing information, then sign in.

![DigitalOcean home page](https://www.dropbox.com/s/wsbuv3t2ho83jmu/Screenshot%202015-01-25%2012.55.03.png?dl=1)

<small>[&#65514; Back to Top](#top)</small>

### <a name="digitalocean-add-ssh"></a>Adding Your SSH Key

1. Open your terminal and type `pbcopy < ~/.ssh/id_rsa.pub`.  This will copy your public SSH key to your clipboard.
2. On DigitalOcean's control panel (assuming you've already logged into the account you just created), click the [SSH Keys](https://cloud.digitalocean.com/ssh_keys) link on the side.
3. Click "Add SSH Key"
4. For the name, put in whatever the name of the computer you're using is (e.g. someuser-laptop)
5. In the "Public SSH Key" textarea, paste in the key that you copied from step 1.
6. Click "Create SSH Key"
7. Celebrate.

<small>[&#65514; Back to Top](#top)</small>

### <a name="digitalocean-set-up-droplet"></a>Set Up Droplet

On the [Droplets](https://cloud.digitalocean.com/droplets) page, click [Create Droplet](https://cloud.digitalocean.com/droplets/new)
![Create the Droplet](https://www.dropbox.com/s/7rg7p3khy5mqr4g/Screenshot%202015-01-25%2013.11.02.png?dl=1)

<small>[&#65514; Back to Top](#top)</small>

#### Droplet options
##### <a name="digitalocean-pricing-regions"></a>Pricing & Regions

How much you pay a month determines the hardware specifications of your server.  The more you pay, the more resources your server will have to deliver your website to more users.  You can start small, then add additional resources in the future if the need arises.

Each server is located in a specific region on the globe.  Selecting the region that is closest to your customers can help improve their load time.  If you require other options (see below) though, your options might be slightly limited as not all regions have the same features.

<small>[&#65514; Back to Top](#top)</small>

##### <a name="digitalocean-private-networking"></a>Private Networking

Private Networking allows to have a Droplet only accessable via the private network in the data center (region) where your server is located.  This can be useful for things such as database replications.

We will not be using Private Networking in the scope of this guide (yet), but to learn more, read here: [Private Networking](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-use-digitalocean-private-networking)

<small>[&#65514; Back to Top](#top)</small>

##### <a name="digitalocean-user-data"></a>User Data

The User Data setting allows for the Droplet to use ambiguous user data upon its creation.  This is usually used for [CloudInit](https://help.ubuntu.com/community/CloudInit), in which special scripts can be run upon the initial creation of the server - such as configuration settings.

Read More: [What is User Data?](https://www.digitalocean.com/community/tutorials/an-introduction-to-droplet-metadata)

<small>[&#65514; Back to Top](#top)</small>

##### <a name="digitalocean-ipv6"></a>IPv6

What is IPv6?
> IPv6 is the most recent version of the IP protocol that the entire internet relies on to connect to other locations (IP protocol is a bit redundant because IP stands for internet protocol, but we will use it because it is easy). While IPv4 is still in use in many areas of the world, the IPv4 address space is being consumed at a rapid rate and it is not large enough to sustain the rapid deployment of internet-ready devices.
>
> IPv6 looks to solve these problems. As well as making general improvements on the protocol, the most obvious benefit of utilizing IPv6 addresses is that it has a much larger address space. While IPv4 allowed for 2^32 addresses (with some of those reserved for special purposes), the IPv6 address space allows for 2^128 addresses, which is an incredible increase

Whether or not your website supports IPv6 is totally up to you, you framework, or anything else you're using to run the server.

Read More: [Setting up IPv6](https://www.digitalocean.com/community/tutorials/how-to-configure-your-droplet-to-only-use-ipv6-networking)

<small>[&#65514; Back to Top](#top)</small>

##### <a name="digitalocean-backups-snapshots"></a>Backups & Snapshots

> DigitalOcean users have the option of backup and snapshot features. The main difference between the two is that snapshots can be generated manually and can be enabled at any time, while backups are run automatically every few days, depending on the droplet’s disk size, and must be enabled during the droplet's creation.

If you want to keep your server and data backed up, you can choose to enable backups.  NOTE: This costs!  20% of your monthly charge to be exact.  Backups happen automatically without shutting down your server.  **Backups must be enabled when your Droplet is first created**.  Snapshots, on the other hand, can be generated _after_ your server has been created.  However, snapshots are manual.  The current pricing for snapshots is $0.02/GB used for snapshot.

Read More: [What are Backups?  What are Snapshots?](https://www.digitalocean.com/community/tutorials/digitalocean-backups-and-snapshots-explained)

<small>[&#65514; Back to Top](#top)</small>

#### <a name="digitalocean-image"></a>Choosing an Image

For this guide, we will be using the **Ubuntu 14.04 x64** Distribution for the image.  However, DigitalOcean has many different options for server distributions, one-click application installs, and more.

Read More: [One-click App Installs, Distributions, and More](https://www.digitalocean.com/features/one-click-apps/)

<small>[&#65514; Back to Top](#top)</small>

#### <a name="digitalocean-select-ssh-key"></a>Select SSH Key

Click the SSH key that we added earlier, that's it!

For reference: [Adding the Key to Your DigitalOcean Droplet](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets)

<small>[&#65514; Back to Top](#top)</small>

#### <a name="digitalocean-launch"></a>Launching the Server

_This is where the magic happens._

Click "Create Droplet".

### Set DNS
#### <a name="dns-add-your-domain"></a>Add your domain

Now that your Droplet has been created, we need to point your domain to it so the world can access your creation!

Pointing your domain to your Droplet varies depending on whom you purchased your domain through.  All that it requires is setting the domain's [Name Servers](http://www.networksolutions.com/support/what-is-a-domain-name-server-dns-and-how-does-it-work/) to the ones provided through DigitalOcean.

Read More: [How to point your domain to DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars)

<small>[&#65514; Back to Top](#top)</small>

--

# Server Setup Part 2: Installing Tools & Adjusting Settings
## <a name="connect-via-ssh"></a>Connecting via SSH

In order to access our server, we need to connect via SSH.  This is done through the terminal or command line and allows us to run commands on our server from our computer.  Once connected, we can run some commands that allow us to manage our server, install software, reboot, and more.

Some common examples include:

- Listing files and directories: `ls`
- Changing your current directory: `cd /some_directory`
- Copying files: `cp file_you_want_to_copy.txt /destination/directory`
- Moving files: `mv file_you_want_to_move.txt /destination/and_new_file_name_if_wanted.txt`
- Rebooting your server: `sudo reboot`

Read More: [Connecting to your Droplet via SSH](https://www.digitalocean.com/community/tutorials/how-to-connect-to-your-droplet-with-ssh)
[Setting up Users](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04)

<small>[&#65514; Back to Top](#top)</small>

## <a name="installing-dokku-alt"></a>Installing dokku-alt

dokku-alt is a software package that allows for the ability to push your code to your server with Git and has many other features.  It's where the "Build Your Own Heroku" name comes from, as Dokku recreates some of the most popular features from Heroku, such as git deploys and buildpacks.

dokku-alt adds additional features to dokku, such as the dokku-alt-manager, which we'll be using.

Installing dokku-alt is easy and fully automated.  It can be done as follows:

1. SSH into your server from your terminal: `ssh root@yourdomain`
2. Run the following command: `sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/dokku-alt/dokku-alt/master/bootstrap.sh)"`
3. After the installation has completed, visit `yourdomain:2000` to finish set up.
	- Set the domain name where you website will be accessed
	- Choose how you want users to access your apps on that domain (myapp.yourdomain.com or yourdomain.com/myapp)
4. Save, then close out of your SSH session.

Read More: [dokku-alt](https://github.com/dokku-alt/dokku-alt/)

<small>[&#65514; Back to Top](#top)</small>

## <a name="setting-up-virtual-memory"></a>Setting up Virtual Memory

You may come to find that you run out of memory quickly, depending on how many visitors visit your website or what services your server will be running.  In fact, DigitalOcean's smallest memory tier (512MB at the time of this writing) can quickly fill up just running the server, let alone deploying code via git and more.

To fix the "Out of Memory" errors, we can create virtual memory on your server's SSD.  This allocates a certain amount of space of your server's SSD to be used as a virtual "RAM".  However, writing data to the SSD this way can be "costly" in terms of how long it takes to do.

<small>[&#65514; Back to Top](#top)</small>

### <a name="bring-in-the-swap"></a>Bring in the SWAP

We'll be looking at the following guide for setting up a SWAP - virtual memory to be used in case your actual memory runs out when performing a task.

Read More: [SWAP](https://www.digitalocean.com/community/tutorials/how-to-configure-virtual-memory-swap-file-on-a-vps)
[Ubuntu 14.04 SWAP](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04)

<small>[&#65514; Back to Top](#top)</small>

## <a name="using-dam"></a>Using dokku-alt-manager (dam) to create your apps and databases

In order to deploy apps and databases, we can either create them via command line or use the dokku-alt-manager interface that handles it for us.

To install dokku-alt-manager:

1. SSH into your server: `ssh root@yourdomain`
2. Run the install command: `dokku manager:install`
3. Visit `http://dam.yourdomain.com` or `http://yourdomain.com/dam` (depending on how you chose to do the routing when installing dokku-alt)
4. Your host (Droplet) should be listed, which you can select and deploy an app to or add a database.

--

<small>[&#65514; Back to Top](#top)</small>

# <a name="deploying-code"></a>Deploying Code

**NOTE: In order to deploy code, you must have set up an app in the previous step.**

<small>[&#65514; Back to Top](#top)</small>

## <a name="initial-git-setup"></a>Initial git setup & first deploy

Now that your app has been set up and everything you need has been installed, we can deploy our app to the server!  Remember, this is a Heroku clone, so Heroku requirements and necessary here, such as a valid [Procfile](https://devcenter.heroku.com/articles/procfile) and [Buildpack](https://devcenter.heroku.com/articles/buildpacks).

For this example, we'll use the Node.js Heroku sample app.

```
$ git clone https://github.com/heroku/node-js-sample
$ cd node-js-sample
$ git remote add dokku dokku@yourdomain.com:your-app-name
$ git push dokku master
```

Your app should now be online! (yourapp.yourdomain.com)

<small>[&#65514; Back to Top](#top)</small>

## <a name="ch-ch-ch-ch-changes"></a>Making changes and pushing them to the interweb

Now that your app is online, you will most likely need to make changes.  To do that, you will make your changes, add the changes to Git (i.e. stage them), commit your changes when you've reached a point where the work is completed, then push those changes to your Droplet.  This is all done through the command line:

```
# in your working directory
$ git add .
$ git commit
$ git push dokku master
```

Sometimes you may need to add a file that doesn't already exist in the git repository (repo).  Or maybe you only want to add one or two files and not all the changes you've made.  These can both be accomplished by either of these options:

- `git add -A` adds all modified files as well as untracked, unadded files.
- `git add app/controllers/yourfile.rb` adds a specific file or files (separated by a space).

<small>[&#65514; Back to Top](#top)</small>

# <a name="the-end"></a>Conclusion

Now you should have all you need to get your app online.  What's next?

- Get comfortable with Git and how to revert changes in the event a commit breaks something
- Explore the one-click app install options
- Try setting up a Droplet from a snapshot or backup that you've previously created
- Try creating a database replication Droplet and connect to it via the private network
- Create an install script to be used with CloudInit and user data to automate the server initialization configuration

<small>[&#65514; Back to Top](#top)</small>

