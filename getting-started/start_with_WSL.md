# Set up your working environment

This page is intended for Windows users who want to contribute more substantial changes to documentation (or on a more regular basis) using the Ubuntu command line. For quick corrections to a page, it's perfectly fine to use the GitHub web interface instead.

## Why are we using WSL?

WSL is the Windows Subsystem for Linux. It sets aside some space on your computer and creates a virtual Ubuntu machine in that space. This will allow you to work as if you're using an Ubuntu computer, but without having to change anything on your Windows system. Even if you somehow break your Ubuntu virtual machine (VM), the Windows machine won't be harmed, so WSL provides a safe way to become more familiar with using Linux!

## Install VS Code on your Windows machine

It might seem strange to install VS Code before we install WSL, but by doing things this way round we'll save ourselves several fiddly steps later!

You will need a text editor to be able to make changes to the content you're working with, and the best editor is VS Code. It can be installed on your windows machine, and then WSL can use it – even from inside your Ubuntu virtual machine.

Go to the [Visual Studio code website](https://code.visualstudio.com/) and scroll down until you see the download button for Windows. 

![Download options for VS Code](images/install_VSCode.png)

It may automatically detect your operating system and download VS Code if you're on Windows. If it doesn't start automatically, click on the Windows download button and run the installer when it's done. 

The installation should be straightforward, and the default options should work just fine.

## Install WSL on your Windows machine

We can install WSL using the Windows command line. To do this, open the Windows Terminal by clicking on the Windows start menu and scrolling down to Terminal (or searching for "Terminal" in the search bar).

Once you have opened the Terminal window, type the following, then press enter to run it:

```
wsl --install
```

When asked if you want to allow WSL to make changes to your device, click "yes".

After WSL has finished installing, you'll need to restart your computer (your physical machine, not just the Terminal window) before you can continue.

## Your new Ubuntu VM

After you've restarted your physical machine, WSL will automatically launch Ubuntu in a new Terminal window. You won't need the Windows Terminal anymore, so you can close that one, and leave only the Ubuntu window open.

The Ubuntu window will be asking you to set up a username and password. This is separate to the credentials you use to log into your Windows machine, although you can choose the same. The password you type will be completely "hidden", so you won't be able to see it or see how many characters you've typed. It will ask you to confirm the password anyway, so if you think you made a mistake, you'll still have to type it correctly twice.

After this is done, your Ubuntu VM will be ready to use!

## Update the virtual Ubuntu machine

It's always a good idea when you start up a new virtual machine to update and upgrade it, which you can do with the following command typed into the Ubuntu terminal:

```
sudo apt update && sudo apt upgrade
```

Using `sudo` will prompt you for a password. This is the same password as you entered when you set up your Ubuntu machine in the previous step.

When asked if you want to continue, type `y` (not case sensitive).

You're now ready to move onto the next stage! [Install and configure git](install_git.md).
