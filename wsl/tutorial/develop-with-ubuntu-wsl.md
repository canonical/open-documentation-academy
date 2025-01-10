# Develop with Ubuntu on WSL

The easiest way to access your Ubuntu development environment in WSL is by using Visual Studio Code via the built-in `Remote` extension.

## What you will learn

* How to install WSL and Ubuntu on WSL from the terminal
* How to set up Visual Studio Code for remote development with Ubuntu on WSL
* How to create a basic Node.js webserver on Ubuntu using Visual Studio Code
* How to preview HTML served from an Ubuntu WSL instance in a native browser on Windows

## What you will need

* A PC with Windows 10 or 11

## Install Ubuntu on WSL2

### Install WSL

Open a PowerShell prompt as an Administrator and run:

```{code-block} text
> wsl --install
```

This command will enable the features necessary to run WSL and also install the latest Ubuntu distribution available for WSL. It is recommended to reboot your machine after this initial installation to complete the setup.

### Install Ubuntu on WSL

WSL supports a variety of Ubuntu releases. Check our [reference on the distributions](https://documentation.ubuntu.com/wsl/en/latest/reference/distributions/) for more information.

There are multiple ways of installing Ubuntu on WSL, here we focus on using the terminal.
For other installation methods, refer to your dedicated guide:

* [Install Ubuntu on WSL2](https://documentation.ubuntu.com/wsl/en/latest/guides/install-ubuntu-wsl2/)

In a PowerShell terminal, run `wsl --list --online` to see all available distros and versions:

```{code-block} text
The following is a list of valid distributions that can be installed.
The default distribution is denoted by '*'.
Install using 'wsl --install -d <Distro>'.

  NAME                                   FRIENDLY NAME
* Ubuntu                                 Ubuntu
  Debian                                 Debian GNU/Linux
  kali-linux                             Kali Linux Rolling
  Ubuntu-18.04                           Ubuntu 18.04 LTS
  Ubuntu-20.04                           Ubuntu 20.04 LTS
  Ubuntu-22.04                           Ubuntu 22.04 LTS
  Ubuntu-24.04                           Ubuntu 24.04 LTS
...

``` 

Your list may be different once new distributions become available.  

You can install a version using a NAME from the output, for example:

```{code-block} text
> wsl --install -d Ubuntu-24.04
```

You'll see an indicator of the installation progress in the terminal:

```{code-block} text
Installing: Ubuntu 24.04 LTS
[==========================72,0%==========                 ]
```

Use `wsl -l -v` to see all your currently installed distros and the version of WSL they are using:

```{code-block} text
  NAME            STATE           VERSION
  Ubuntu-20.04    Stopped         2
* Ubuntu-24.04    Stopped         2
```

## Install Visual Studio Code on Windows

One of the advantages of WSL is that it can interact with the native Windows version of Visual Studio Code using its remote development extension.

To install Visual Studio Code, visit the Microsoft Store and search for Visual Studio Code.

Then click **Install**.

![Installation page for Visual Studio Code on the Microsoft Store.](https://github.com/ubuntu/wsl/blob/main/docs/tutorials/assets/vscode/msstore.png?raw=true)

Alternatively, you can install Visual Studio Code from the web link [here](https://code.visualstudio.com/Download).

![Visual Studio Code download page showing download options for Windows, Linux, and Mac.](https://github.com/ubuntu/wsl/blob/main/docs/tutorials/assets/vscode/download-vs-code.png?raw=true)

During installation, under the 'Additional Tasks' step, ensure the `Add to PATH` option is checked.

![Visual Studio Code's "Additional Tasks" setup dialog with the "Add to Path" and "Register Code as an editor for supported file types" options checked.](https://github.com/ubuntu/wsl/blob/main/docs/tutorials/assets/vscode/aditional-tasks.png?raw=true)

Once the installation is complete, open Visual Studio Code.

## Install the Remote Development Extension

Navigate to the `Extensions` menu in the sidebar and search for `Remote Development`.

This is an extension pack that allows you to open any folder in a container, remote machine, or in WSL. Alternatively, you can just install `Remote - WSL`.

![Installation page for the Remote Development Visual Studio Code extension.](https://github.com/ubuntu/wsl/blob/main/docs/tutorials/assets/vscode/remote-extension.png?raw=true)

Once installed we can test it out by creating an example local web server with Node.js

## Install Node.js and create a new project

Open your WSL Ubuntu terminal and ensure everything is up to date by typing:

```{code-block} text
$ sudo apt update
```

Then:

```{code-block} text
$ sudo apt upgrade
```

Entering your password and pressing `Y` when prompted.

Next, install Node.js and npm:

```{code-block} text
$ sudo apt-get install nodejs
$ sudo apt install npm
```

Press `Y` when prompted.

Now, create a new folder for our server.

```{code-block} text
$ mkdir serverexample/
```

Then navigate into it:

```{code-block} text
$ cd serverexample/
```

Now, open up your folder in Visual Studio Code, with the following command:

```{code-block} text
$ code .
```

The first time you do this, it will trigger a download for the necessary dependencies:

![Bash snippet showing the installation of Visual Studio Code Server's required dependencies.](https://github.com/ubuntu/wsl/blob/main/docs/tutorials/assets/vscode/downloading-vscode-server.png?raw=true)

Once complete, your native version of Visual Studio Code will open the folder.

## Creating a basic web server

In Visual Studio Code, create a new `package.json` file and add the following text ([original example](https://learn.microsoft.com/en-gb/archive/blogs/cdndevs/visual-studio-code-and-local-web-server#3-add-a-packagejson-file-to-the-project-folder))

```{code-block} json
{
    "name": "Demo",
    "version": "1.0.0",
    "description": "demo project.",
    "scripts": {
        "lite": "lite-server --port 10001",
        "start": "npm run lite"
    }, 
    "author": "",
    "license": "ISC",
    "devDependencies": {
        "lite-server": "^1.3.1"
    }
}
```

Save the file and then, in the same folder, create a new one called `index.html`

Add the following text, then save and close:

```{code-block} html
<h1>Hello World</h1>
```

Now return to your Ubuntu terminal (or use the Visual Studio Code terminal window) and type the following to install a server defined by the above specifications detailed in `package.json`:

```{code-block} text
$ npm install
```

Finally, type the following to launch the web server:

```{code-block} text
$ npm start
```

You can now navigate to `localhost:10001` in your native Windows web browser by using `CTRL+LeftClick` on the terminal links.

![Windows desktop showing a web server being run from a terminal with "npm start", A Visual Studio Code project with a "hello world" html file, and a web browser showing the "hello world" page being served on local host.](https://github.com/ubuntu/wsl/blob/main/docs/tutorials/assets/vscode/hello-world.png?raw=true)

That’s it!

By using Ubuntu on WSL you’re able to take advantage of the latest Node.js packages available on Linux as well as the more streamlined command line tools.

## Enjoy Ubuntu on WSL!

In this tutorial, we’ve shown you how to connect the Windows version of Visual Studio Code to your Ubuntu on WSL filesystem and launch a basic Node.js webserver.

We hope you enjoy using Ubuntu inside WSL. Don’t forget to check out our other tutorials for tips on how to optimise your WSL setup for Data Science.

### Further Reading

* [Install Ubuntu on WSL2](../howto/install-ubuntu-wsl2.md)
* [Microsoft WSL Documentation](https://learn.microsoft.com/en-us/windows/wsl/)
* [Setting up WSL for Data Science](https://ubuntu.com/blog/wsl-for-data-scientist)
* [Ask Ubuntu](https://askubuntu.com/)
