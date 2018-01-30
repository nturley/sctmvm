# Starcraft Tournament Manager Virtual Machine
Setting an VM environment to run BWAPI bots can be a little tricky. Here's a fast overview of what I've learned.

## Why would I want to do this?
* You want to develop a BWAPI bot but you don't have a physical Windows machine
* You want an automated test for your bot as you are developing. It's easy to make assumptions that aren't true for all maps or all opponents and it's slow to manually set up test runs against a variety of maps and opponents.
* You want to train your bot against a variety of opponents to find strategic gaps in your bot.

## Windows Virtual Machine
There are many VM applications. I like virtualbox.

https://www.virtualbox.org/wiki/VirtualBox

Microsoft distributes free virtual machines for development and testing, mostly for IE and Edge. Presumably we can use these to test BWAPI bots as well.

https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/

## Starcraft : Broodwar

Broodwar is now free to play. Blizzard distributes version 1.18 or so which is incompatible with BWAPI. We need Broodwar 1.16.1. ICCUP distributes a zip file with 1.16.1 that can be unzipped and run as a standalone executable.

https://iccup.com/sc_start.html

We'll need a copy on the client and on the server

## Tournament Manager
We will need to put the tournament manager files on the client and the server, Unzip the latest bots, Execute the vcredists installers.

https://github.com/davechurchill/StarcraftAITournamentManager

## Provisioning
Running windows installers is painful. Chocolatey is a windows package manager that can execute many application installs for you. It's super nice.

https://chocolatey.org/

## VM setup steps
You might need to restart when the VM first comes up.

### Install chocolatey
open an administrator command shell.
```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```
then close this command shell and open another one

### Install git, 7zip, curl, jdk8, vcredists
```
cd C://Users/IEUser/Documents
choco install git -y
choco install jdk8 -y
choco install 7zip -y
choco install curl -y
refreshenv
curl -O http://www.cs.mun.ca/%7Edchurchill/starcraftaicomp/all_vcredist_x86.zip
7z x all_vcredist_x86.zip -ovcredists
cd vcredists
intall_all_vcredists.bat
cd ..
```

### Download Starcraft
download the starcraft executable. Run setup.exe to add the path to the registry so BWAPI can find it.
```
curl -O http://files.theabyss.ru/sc/starcraft.zip
7z x starcraft.zip -ostarcraft
cd starcraft
setup.exe
cd ..
```

### Download Tournament Module
```
git clone https://github.com/davechurchill/StarcraftAITournamentManager
cd StarcraftAITournamentManager/server/bots
7z x aiide_2016_bots.7z -o./
cd ../../..
```

### Registry Permissions
I'm still a little hazy on this point but apparently, we need to edit our registry permissions.
The tournament instructions state: ```Edit registry so that HKLM\SOFTWARE has "Full Control" for current user```

### Launch server
```
cd StarcraftAITournamentManager/server
run_server.bat
```
The server will kick up a Swing GUI and ask you two questions. At some point, I'll have to figure out how to script the server configuration so it doesn't have to ask anything.

Windows Firewall might also ask if it's okay if Java has access to the network

### Launch client
edit the json file in StarcraftAITournamentManager/client/clientsettings.json to the ip address of the server (one of them can just be localhost) and change the starcraft directory to "C:\\Users\\IEUser\\Documents\\starcraft\\"
