# Introduction

This file contains all of the images associated with specific Ubuntu WSL documentation pages.

You will use this file to suggest alternative (alt) text for images in the documentation.
Alt text helps people who are visually-impaired or have a visual disability to interpret images using screen reader technology.
A lack of alt text or low-quality alt text is an accessibility issue that can make documentation difficult or impossible to navigate.

If you are struggling to create alt text for an image it might be
useful to go to the documentation and view the image in context.
The live docs that include these images can be found below:

[Official Ubuntu WSL docs](https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/)

# Image accessibility and alt text

Images in this file are defined using conventional markdown syntax.
When you edit this file, images will be prefixed with `!` followed by a pair of square brackets and a pair of parentheses. The alt text will be found in square brackets `[here]` and the url to the image will be in parentheses `(here)`:

```markdown
![this is alt text](url)
```

The square brackets in the Ubuntu WSL docs currently contain image dimensions, such as `[|624x489]`, which is not valid alt text.

Any alt text should fit the following criteria:

- Describe the content of the image clearly and in as few words as possible
- Preferably be one sentence in length with minimal punctuation
- Avoid repetition and include only the essential information once
- Avoid unecessary words, such as "image of..." (the user will know it's an image)

It is difficult to create good alt text for certain images.
An example is an image of a terminal that contains a lot of text.
You would not reproduce all of that text but instead attempt to
explain what it is and what it does:

> Bash snippet showing the updating of packages.

> Log of output showing successful running of script.

# Table of contents

- [Introduction](#introduction)
- [Image accessibility and alt text](#image-accessibility-and-alt-text)
- [Table of contents](#table-of-contents)
- [Tutorials](#tutorials)
  - [Working with Visual Studio Code](#working-with-visual-studio-code)
  - [Windows and Ubuntu Interoperability](#windows-and-ubuntu-interoperability)
  - [Run a .NET Echo Bot as a systemd service on Ubuntu WSL](#run-a-net-echo-bot-as-a-systemd-service-on-ubuntu-wsl)
  - [Enabling GPU acceleration with the NVIDIA CUDA platform](#enabling-gpu-acceleration-with-the-nvidia-cuda-platform)
  - [Use WSL for data science and engineering](#use-wsl-for-data-science-and-engineering)
- [Howtos](#howtos)
  - [Install Ubuntu on WSL2](#install-ubuntu-on-wsl2)

# Tutorials

## Working with Visual Studio Code

[Link to full documentation page](https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/tutorials/vscode/) 

![Installation page for Visual Studio Code on the Microsoft Store](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/vscode/msstore.png?raw=true)

As an example, alt text for the image above could be: `Install page for Visual Studio Code on the Microsoft Store`.

![Visual Studio Code download page showing download options for Windows, Linux (.deb and .rpm packages), and Mac](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/vscode/download-vs-code.png?raw=true)

![Visual Studio Code's installation "Additional Tasks" Steps with the "Add to PATH" and "Register Code as an editor for supported file types" options checked](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/vscode/aditional-tasks.png?raw=true)

![Installation page for the Remote Development Visual Studio Code extension](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/vscode/remote-extension.png?raw=true)

![Bash snippet showing the installation of VS Code Server's required dependencies](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/vscode/downloading-vscode-server.png?raw=true)

![Windows desktop showing a terminal running "npm start", a Visual Studio Code window with project files, and a web browser displaying 'Hello World' and 'Tutorial complete!' at http://localhost:10001](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/vscode/hello-world.png?raw=true)

## Windows and Ubuntu Interoperability

[Link to full documentation page](https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/tutorials/interop/)

![Jupyter Notebook running in a web browser at http://localhost:8888, showing an empty notebook list](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/interop/jupyter.png?raw=true)

![Jupyter interface showing the creation of a new Python 3 notebook. The 'New' button is clicked, revealing a dropdown menu with 'Python 3' highlighted](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/interop/jupyter-python.jpg?raw=true)

![Python script in a Jupyter notebook](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/interop/jupyter-script.png?raw=true)

![Screenshot of Windows file explorer](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/interop/ubuntu-home.png?raw=true)

![Windows desktop showing terminal, PowerShell, and LibreOffice Calc with file type statistics table and pie chart](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/interop/spreadsheet.png?raw=true)

## Run a .NET Echo Bot as a systemd service on Ubuntu WSL

[link to documentation page](https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/tutorials/dotnet-systemd/)

![Selecting Bot Framework Echo Bot from Dotnet new --list](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/templates.png?raw=true)

![Bash snippets confirming that Echo bot was installed and setup correctly](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/welcome-to-dotnet.png?raw=true)

![Windows desktop showing the Echo bot app running at localhost:3978](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/your-bot-is-ready.png?raw=true)

![Bot Framework Emulator homepage](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/bot-framework-emulator.png?raw=true)

![ipconfig command output showing network adapter details, including IPv4 addresses for Wi-Fi and WSL.](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/ipconfig.png?raw=true)

![Bot Emulator settings page](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/emulator-settings.png?raw=true)

![Bot configuration page](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/open-a-bot.png?raw=true)

![Live chat with Echo bot](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/start-chatting.png?raw=true)

![](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/program-cs.png?raw=true)

![Results of running "systemctl status echoes.service" on terminal](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/systemctl-status-inactive.png?raw=true)

![Results of running "sudo systemctl" on terminal](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/systemctl-status-running.png?raw=true)

![Live chat with Echo bot](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/dotnet-systemd/start-chatting-service.png?raw=true)

## Enabling GPU acceleration with the NVIDIA CUDA platform

[link to documentation page](https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/tutorials/gpu-cuda/#)

![Install support for Linux GUI apps page on WSL documentation](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/install-drivers.png?raw=true)

![Windows file explorer showing the downloaded NVIDIA GPU driver for WSL](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/downloads-folder.png?raw=true)

![Windows Package Installer confirmation page for NVIDIA Package Launcher](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/nvidia-allow-changes.png?raw=true)

![Default directory confirmation page for NVIDIA Display Driver](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/default-dir.png?raw=true)

![NVIDIA Display Driver installation progress screen](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/please-wait-install.png?raw=true)

![NVIDIA Graphics Driver startup page](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/splash-screen.png?raw=true)

![NVIDIA software license agreement](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/license.png?raw=true)

![NVIDIA Graphics Driver installation options with "Express" checked](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/installation-options.png?raw=true)

![NVIDIA Virtual Host controller installation progress](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/installing.png?raw=true)

![NVIDIA Graphics Driver installation success page](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/install-finished.png?raw=true)

![Terminal output showing successful installation of NVIDIA CUDA toolkit on Ubuntu](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/done-done.png?raw=true)

![Terminal output showing the successful compilation of a CUDA sample application](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/make.png?raw=true)

![Terminal output showing the results of running the deviceQuery sample application](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/gpu-cuda/device-query.png?raw=true)

## Use WSL for data science and engineering

[link to documentation page](https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/tutorials/data-science-and-engineering/)

![Windows desktop showing a terminal running "octave --gui" and the Octave GUI](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/data-science-engineering/octave.png?raw=true)

![Octave GUI showing the "Save and Run button" for the juliatest.m file](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/data-science-engineering/save-file.png?raw=true)

![A Julia fractal](https://github.com/ubuntu/WSL/blob/main/docs/tutorials/assets/data-science-engineering/julia-fractal.png?raw=true)

# Howtos

## Install Ubuntu on WSL2

[Link to documentation page](https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/guides/install-ubuntu-wsl2/)

![Installation page for Ubuntu 24.04 LTS on the Microsoft store](https://github.com/ubuntu/WSL/blob/main/docs/guides/assets/install-ubuntu-wsl2/choose-distribution.png?raw=true)

![Search results fpr Ubuntu 24.04 LTS on Windows search bar](https://github.com/ubuntu/WSL/blob/main/docs/guides/assets/install-ubuntu-wsl2/search-ubuntu-windows.png?raw=true)
