BYOH
====

![BYOH logo](logo.png)

### BYOH: Build Your Own Heroku
_A Getting Started Guide for Complete Noobs... like me_

--


#### Table of Contents
1. What is this about?
- Who is this for?
- Why should I even?
- What's included?
- Server Setup Part 1: Creating you server
	- Prerequisites
		- Domain Name
		- Git
			- What is Git?
			- Why should I use it?
		- SSH
			- What is SSH?
			- Generating SSH Keys
	- Setting up a DigitalOcean Droplet
		- Sign Up
		- Set Up Droplet
			- Droplet options
				- Private Networking
				- User Data
				- IPv6
				- Backups & Snapshots
			- Adding SSH
			- Launching the Server
		- Set DNS
			- Add your domain
- Server Setup Part 2: Installing Tools & Adjusting Settings
	- Connecting via SSH 
	- Installing Dokku-alt
	- Setting up Virtual Memory
		- Bring in the SWAP
- Deploying Code
	- Initial git setup & first deploy
	- Making changes and pushing them to the interweb

--

# What is this about?

Everything is online, from retail stores and shops to streaming video and social networks. This guide is intended to help readers understand how a website or web app can be put online and made available for others to access.  

# Who is this for?

The target audience for this guide is intentially broad.  It gives you enough overview for those that are just curious about the process, as well as granular details into how to do it yourself.

# Why should I even?

Sure, there are many different ways of getting your website put onto the internets - some even easier than this.  But traditional web hosts can be problematic and a standard web hosting solution isn't usually a valid representation of how companies are getting their sites & apps online.  

Many web hosts still require you to manually upload all your code & changes to their servers, but what happens if you accidentally upload the wrong file or delete something you didn't mean to?  With BYOH, readers are shown ways to prevent that from happening or even undoing something that was done accidentally.

# What's involved?

This guide will show you how to set up a server in the cloud (via DigitalOcean) to host your website or app so that others can access it on the internet.

# Server Setup Part 1: Creating you server
## Prerequisites
### Domain Name

A [domain name](http://netforbeginners.about.com/od/d/f/domain_name.htm) is what you type into your [browser](http://www.gcflearnfree.org/internet101/4)'s address bar to go to a website, such as _[www.google.com](http://www.google.com)_, _[whitehouse.gov](http://whitehouse.gov)_, or _[theroadhome.org](theroadhome.org)_.  Behind the scenes, the domain actually takes you to an [IP Address](http://computer.howstuffworks.com/internet/basics/question549.htm) where your server is located.  It makes it easier for others to access your website.  Can you imagine if we had to know every IP address for all of the websites we've visited?  Yikes.

Getting your very own domain name is easy and fairly inexpensive.  All that's required is [buying your domain name](https://www.namecheap.com/), then pointing it to your server (which will be covered below).

### Git

_Or "how to undo that terrible change you just made that broke everything"_

#### What is Git?

[Git](http://www.makeuseof.com/tag/git-version-control-youre-developer/) is a way to keep a history of changes that have been made to your code.  It allows for multiple developers to work on the same code simultaneously without one person overwriting work that another had already done.  It makes it easier to resolve any conflicts in the code before uploading it to the server to be put online.  

Git (or some other version control software) is critical when working on code with more than just one person.  However, even if you are the only one writing code, it is still important to use in case you need to undo a change you made or figure out at what point in history that a piece of code was written.

#### Why should I use it?

Imagine you are working on changing the homepage to your website.  You create a new [index.html](http://webdesign.about.com/od/beginningtutorials/f/index_html.htm) file, fill it with some junk content, and maybe even throw in a couple irrelevant pictures as placeholders.  Then you decide to scrap it, but you don't have the previous version of the file.  So you [connect to your server](https://help.yahoo.com/kb/yahoo-web-hosting/ftp-sln20433.html) to download the current version and overwrite your "work in progress".  You click the download button, click "overwrite", and get the file.  Or so you thought...  When you open up the "downloaded" file, you realize it's the same file you were working on before!  You suddenly realize that you accidentally clicked the _Upload_ button instead of _Download_.  Now, the whole world can see your junky work in progress!  And you don't have the original file anywhere to go back!  Time to call tech support...  Hope you have a couple hours to spare!

Now, what if you had used Git?  If you wanted to make the same mistake, you would have to 1.) add the file, 2.) commit your change, and 3.) push it to the server.  Hopefully you would have caught your mistake before you pushed it out.  But what if you didn't?  With Git, you have an easy way to undo that change you made, no tech support required!  You can revert your change and push it out and everything will be back to where it was before, when it was working!

Or what if you had multiple people working on the same file at the same time?  One person is making numerous, in-depth changes, where the other just had to fix a typo.  The bigger change goes out first, but then the other typo fix overwrites it (using the old version).  The developer making the big changes sees that a new version was uploaded, so they download it and replace their current version... Hopefully they had a backup!

### SSH

_Or "hold onto your butts"_

#### What is SSH?

[SSH](http://searchsecurity.techtarget.com/definition/Secure-Shell) is a way to securely connect to your web server and make changes.  But be careful!  Don't run any command if you aren't 100% sure what it does.  You could delete something you don't mean to or install something that could break or be unsafe for your server.

You use SSH through your computer's [terminal/command line application](http://blog.teamtreehouse.com/introduction-to-the-mac-os-x-command-line).  However, to connect to your server via SSH, you must set up [SSH Keys](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets) if you haven't already (see below).


#### Generating SSH Keys

If you already have SSH keys created and want to use them, you can skip this step.  However, if you don't have any or are unsure, continue on.

[How to Set Up SSH Keys on DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets)

## Setting up a DigitalOcean Droplet
### Sign Up

First, [create an account on DigitalOcean](http://digitalocean.com), add your billing information, then sign in.

![DigitalOcean home page](https://www.dropbox.com/s/wsbuv3t2ho83jmu/Screenshot%202015-01-25%2012.55.03.png?dl=1)

### Adding Your SSH Key

1. Open your terminal and type `pbcopy < ~/.ssh/id_rsa.pub`.  This will copy your public SSH key to your clipboard.
2. On DigitalOcean's control panel (assuming you've already logged into the account you just created), click the [SSH Keys](https://cloud.digitalocean.com/ssh_keys) link on the side.
3. Click "Add SSH Key"
4. For the name, put in whatever the name of the computer you're using is (e.g. someuser-laptop)
5. In the "Public SSH Key" textarea, paste in the key that you copied from step 1.
6. Click "Create SSH Key"
7. Celebrate.


### Set Up Droplet

On the [Droplets](https://cloud.digitalocean.com/droplets) page, click [Create Droplet](https://cloud.digitalocean.com/droplets/new)
![Create the Droplet](https://www.dropbox.com/s/7rg7p3khy5mqr4g/Screenshot%202015-01-25%2013.11.02.png?dl=1)

#### Droplet options
##### Pricing & Regions

How much you pay a month determines the hardware specifications of your server.  The more you pay, the more resources your server will have to deliver your website to more users.  You can start small, then add additional resources in the future if the need arises.

Each server is located in a specific region on the globe.  Selecting the region that is closest to your customers can help improve their load time.  If you require other options (see below) though, your options might be slightly limited as not all regions have the same features.

##### Private Networking
[Private Networking](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-use-digitalocean-private-networking)

##### User Data
[What is User Data?](https://www.digitalocean.com/community/tutorials/an-introduction-to-droplet-metadata)

##### IPv6
[Setting up IPv6](https://www.digitalocean.com/community/tutorials/how-to-configure-your-droplet-to-only-use-ipv6-networking)

##### Backups & Snapshots
[What are Backups?  What are Snapshots?](https://www.digitalocean.com/community/tutorials/digitalocean-backups-and-snapshots-explained)

#### Choosing an Image

For this guide, we will be using the **Ubuntu 14.04 x64** Distribution for the image.  However, DigitalOcean has many different options for server distributions, one-click application installs, and more.

#### Add SSH Key

Click the SSH key that we added earlier, that's it!

For reference: [Adding the Key to Your DigitalOcean Droplet](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets)


#### Launching the Server

_This is where the magic happens._

Click "Create Droplet".

### Set DNS
#### Add your domain
[How to point your domain to DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars)


# Server Setup Part 2: Installing Tools & Adjusting Settings
## Connecting via SSH 
## Installing Dokku-alt
[dokku-alt](https://github.com/dokku-alt/dokku-alt/)
## Setting up Virtual Memory
### Bring in the SWAP
[SWAP](https://www.digitalocean.com/community/tutorials/how-to-configure-virtual-memory-swap-file-on-a-vps)
# Deploying Code
## Initial git setup & first deploy
## Making changes and pushing them to the interweb

	
	
