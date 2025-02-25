# Get started as a contributor

This set of guides is intended for anyone who wants to contribute more substantial changes to documentation (or on a more regular basis) using the Ubuntu command line. 

For quick corrections to a page, it's perfectly fine to use the GitHub web interface instead! 

## Prerequisites

Before you start using this guide, you will need to set up [a GitHub account](https://github.com/), if you don't already have one.
The GitHub documentation is rather good, if you get stuck or need any explanation of particular topics.

### Command-line basics

Working on the command line takes a little getting used to, but doesn't need too much practice to get comfortable with it. Knowing the following commands is enough to get started:

|||
| -- | -- |
| `cd <folder>` | Change directory down to `<folder>` |
| `cd ..` | Go back up one directory level |
| `ls` (or `ls -all`) | Lists all files (including hidden files) |
| `touch <file name>` | Create an empty file called `<file name>` |
| `rm <file name>` | Remove the file called `<file name>` |
| `code <file name>` | Open the named file in VS Code |

Although you can use the sequence:

```bash
touch <file name>
code <file name>
```
To create and then open an empty file, if you use `code <file name>` directly without creating the file first, VS code will create the file for you. 

## Setting up your environment 

If you're using a Windows machine, start by following these instructions:

- [Set up WSL on Windows](start_with_WSL).

If you are working on a project that uses Sphinx to render the documentation, continue with:
  
- [Set up Sphinx](setup_sphinx)

## Using git on the command line

Git is central to the way many developers and technical writers work. It enables us to work on the same project in parallel, and carefully manage and monitor open source contributions.

- [Work with git](using_git)

```{toctree}
:hidden:
:titlesonly:
:maxdepth: 2

Set up WSL on Windows <using_wsl>
Set up Sphinx <setup_sphinx>
Work with git <using_git>
Troubleshooting <troubleshooting>
```
