Date: 31 Jan 2024
=================

Creating a Dotnet 6 Dev Container

Note that I didnt install Docker Desktop anywhere.
Installed WSL2 -> Ubuntu 22.04.3 and installed docker in Ubuntu

Windows Laptop
	Step 1: Create a new WSL distro using the image ubuntu-22.04-default.tar
		wsl --import Ubuntu-22.04-dev02 C:\Prasun\wslDistroStorage\dev02\ C:\Prasun\wsl2-images\ubuntu-22.04-default.tar
	Step 2: wsl -d Ubuntu-22.04-dev02
WSL 
		Setup name server, apt-get update, terminate instance, re-launch, setup wsl.conf, update, terminate, relaunch, reset /etc/resolve.conf, update, terminate, relaunch
	Step 3: apt-get install docker.io docker-compose, exit, terminate, relaunch, update
	Step 4: git clone https://github.com/microsoft/vscode-remote-try-dotnet.git
		reset hard to the commit "4a3b481 Migrate demo app to .NET 6 (#31)"
		Important: In .devcontainer\devcontainer.json, edit the last line and set user="root" instead of "vscode"
Windows Laptop
	Step 5: Open VS Code -> Open a remote window -> Connect to WSL using Distro ..
		When we do this, VS Code installs VS Code Server in the WSL distro
		I tried doing the same by typing "code ." in the WSL terminal but it failed with some certificate missing exception (ZScaler certificate missing etc), so I switched to Windows
		However, once it is installed, we can type "code ." in WSL terminal and it would work fine
		Starting VS Code in Windows and connecting to remote WSL distro is an equivalent step thoughts
	Step 6: VS Code Terminal logs have stopped and I am waiting for a while, it doesnt let me type any new command there
		Disconnect remote, exit VS Code
WSL
	docker ps
	docker exec -it <> bash
	cd /workspaces/..whatever
	dotnet restore
	dotnet msbuild
	dotnet whatever.dll
Windows Laptop
	VS Code recognizes that we executed something, shows popup to open in browser or preview. Use preview (Google Chrome wants a certificate for localhost:3000, but the app we installed doesnt have an HTTPS endpoint

Congratulations !!

Finally, when we have a first dev container running, we export the distro

PS C:\Users\pthapliy> wsl --export Ubuntu-22.04-dev02 C:\Prasun\wsl2-images\ubuntu-22.04-dotnetdevcontainer.tar
Export in progress, this may take a few minutes.
The operation completed successfully.


PS C:\Users\pthapliy> dir C:\Prasun\wsl2-images\
    Directory: C:\Prasun\wsl2-images
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         1/25/2024  10:26 AM           1302 ReadMe.txt
-a----         1/25/2024   9:42 AM     1064407040 ubuntu-22.04-default.tar
-a----         1/31/2024   3:46 PM     3232819200 ubuntu-22.04-dotnetdevcontainer.tar

3 GB image for dotnet

To create a new WSL distro using this image:
wsl --import DotnetDevContainer01 C:\Prasun\wslDistroStorage\DotnetDevContainer01\ C:\Prasun\wsl2-images\ubuntu-22.04-dotnetdevcontainer.tar

wsl -d DotnetDevContainer01

root@DESKTOP-V99LATL:/mnt/c/Users/pthapliy# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
root@DESKTOP-V99LATL:/mnt/c/Users/pthapliy# docker images
REPOSITORY                                                                                      TAG              IMAGE ID       CREATED          SIZE
vsc-vscode-remote-try-dotnet-b22768f37b019d8231511298380e7c5335e0c368f9f1a14e2616a4899f6441e3   latest           2607bedfb8a2   32 minutes ago   1.26GB
mcr.microsoft.com/devcontainers/dotnet                                                          0-6.0-bullseye   4388f56fb016   3 months ago     1.26GB
root@DESKTOP-V99LATL:/mnt/c/Users/pthapliy#

