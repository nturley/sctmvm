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

Unfortunately, these VMs weren't set up very well for vagrant. I opened it up, disabled the firewall, made the network connection private, made a user named vagrant with password vagrant, and enabled remote desktop. Once I'd made all of these changes all of the following commands seemed to work correctly with few or no errors or warnings.

```
vagrant up
vagrant ssh
vagrant winrm
vagrant rdp
vagrant halt
```

Once the VM is clean, it might be useful to package this as a box as a starting point for future projects.

## Starcraft : Broodwar

Broodwar is now free to play. Blizzard distributes version 1.18 or so which is incompatible with BWAPI. We need Broodwar 1.16.1. ICCUP distributes a zip file with 1.16.1 that can be unzipped and run as a standalone executable.

https://iccup.com/sc_start.html

download and unzip to a directory with no spaces and make sure it can launch.

## Provisioning
Running windows installers is painful. Chocolatey is a windows package manager that can execute many application installs for you. It's super nice. You can use it to install chrome, git, a text editor, and the JDK.

https://chocolatey.org/

## Tournament Manager
At this point, you could install BWAPI and use the Chaos Launcher Multi-Instance to play bots against each other, but it's tricky to set up and automate. Thankfully, Dave Churchill has written an open tournament module that can automatically execute tournaments of BWAPI bots. The repo includes many versions of BWAPI, the 2016 AIIDE bots, and a collection of maps. The repo is very well setup. It is very easy to get it up and running.

https://github.com/davechurchill/StarcraftAITournamentManager

