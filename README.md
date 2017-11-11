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
put the tournament manager files on the client and the server. Unzip the latest bots. Execute the vcredists installers.

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
choco install git -y
choco install jdk8 -y
choco install 7zip -y
choco install curl -y
curl -O http://www.cs.mun.ca/%7Edchurchill/starcraftaicomp/all_vcredist_x86.zip
7z x all_vcredist_x86.zip -ovcredists
cd vcredists
intall_all_vcredists.bat
rem ###################################
rem the install script missed these two
rem ###################################
vs2015_vcredist.x86.exe /q /norestart
vs2017_vcredist.x86.exe /q /norestart
cd ..
```

### Download Starcraft
```
curl -O http://files.theabyss.ru/sc/starcraft.zip
7z x starcraft.zip -ostarcraft
```

### Download Tournament Module
```
git clone https://github.com/davechurchill/StarcraftAITournamentManager
cd StarcraftAITournamentManager/server/bots
7z x aiide_2016_bots.7z -o./
cd ../../..
```
