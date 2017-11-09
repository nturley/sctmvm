# Starcraft Tournament Manager Virtual Machine
Setting an VM environment to run BWAPI bots can be a little tricky. Here's a fast overview of what I've learned.

## Why would I want to do this?
* You want to develop a BWAPI bot but you don't have a physical Windows machine
* You want an automated test for your bot as you are developing. It's easy to make assumptions that aren't true for all maps or all opponents and it's slow to manually set up test runs against a variety of maps and opponents.
* You want to train your bot against a variety of opponents to find strategic gaps in your bot.

## Windows Virtual Machine
Vagrant is a VM management tool. I've been learning about it and I think I like it. The documentation is sparse but it's pretty straightforward to use.

https://www.vagrantup.com/intro/index.html

Virtualbox is the default VM provider for vagrant. I like virtualbox.

https://www.virtualbox.org/wiki/VirtualBox

Microsoft distributes free virtual machines for development and testing, mostly for IE and Edge. Presumably we can use these to test BWAPI bots as well.

https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/

I've been learning about vagrant with virtualbox. So I'll be using that for VM management. I used the win10 one, because it's on the vagrant cloud thing, but all of them should work.

Unfortunately, these VMs weren't set up very well for vagrant (it looks like Microsoft did their best but had some trouble with their automation scripts). The main things to do are disable the firewall, made the network connection private, made a user named vagrant with password vagrant, and enabled remote desktop. Several people have made scripts that do this:

https://github.com/mrh1997/vanilla-win7-32bit-vagrantbox/tree/v1.1.0

The above scripts have some questionable steps like setting the keyboard layout to german and installing SSH keys. Maybe skip those steps.

copying around virtual machines tends to eat up your disk space pretty quick, btw so watch out for out of storage space errors.  Eventually you should have a box that does the following without warnings or errors.

```
vagrant up
vagrant ssh
vagrant winrm
vagrant rdp
vagrant halt
```

That's your base image. Now make a directory for the client and server. Instead of putting our files solely in the VM, we are going to put them in our synced folders so after we are done we can destroy the VMs but keep the files so we can reproduce this next time.

## Starcraft : Broodwar

Broodwar is now free to play. Blizzard distributes version 1.18 or so which is incompatible with BWAPI. We need Broodwar 1.16.1. ICCUP distributes a zip file with 1.16.1 that can be unzipped and run as a standalone executable.

https://iccup.com/sc_start.html

We'll need a copy on the client and on the server

## Tournament Manager
put the tournament manager files on the client and the server. Unzip the latest bots. Execute the vcredists installers.

https://github.com/davechurchill/StarcraftAITournamentManager

## Provisioning
Running windows installers is painful. Chocolatey is a windows package manager that can execute many application installs for you. It's super nice. You can use it to install the JDK, for example.

https://chocolatey.org/
