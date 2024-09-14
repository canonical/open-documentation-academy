# Getting started with Sphinx

If your repository uses Sphinx to create nicely rendered documentation, you will need to set up Sphinx on your machine.
This requires a small chain of steps that are needed to install other things, so you'll need to run through these steps in this order. 

## Install Python

If you recently updated your machine (with the command `sudo apt update && sudo apt upgrade`), Python3 may already be installed, but we can check that by running the following command:

```bash
python3 --version
```

If Python3 isn't there, you can run the following command to install it:

```bash
sudo apt install python3
```

## Install pip

Pip often comes installed with Python, but not always. Check if it's installed by running:

```bash
pip --version
```

If it's not installed already, run:

```bash
sudo apt install python3-pip
```

### Install Sphinx

Now you have `pip` installed, you can use it to install Sphinx (and that's the end of our chain!) with the following:

```bash
pip install sphinx
```

You will probably also need to install the Python virtual environment since it's unlikely to be installed by default. To do this, run:

```bash
sudo apt install python3.10-venv
```

This command will install all of the packages that are needed to run a virtual Python environment, but may not have come already bundled with Python3.

## Make a local build of your documentation

As you're working on your documentation, you'll want to check that your edits are having the desired effect. 

### Navigate to the open-documentation-academy folder

If you've been following along on the previous pages, you should be within a folder (possibly called `src`) that contains a sub-folder called `open-documentation-academy`. Let's check where we are first.

Type `ls` on the command line to "list show" all the files and folders you have inside your current folder. If this is a fresh Ubuntu virtual machine, you probably won't have anything yet, except for the `open-documentation-academy` folder that we created when we cloned the repository. Since we have that folder, you can do:

```bash
cd open-documentation-academy 
```

This will put you into the ODA folder that contains all of the working contents of the GitHub repository. 

If you're not sure where you are, you can run:

```bash
cd
ls
cd <directory name>
```

Running `cd` by itself will take you back to the root directory, and then you can use `ls` and `cd` to navigate to where you want to be (inside the `open-documentation-academy` folder).

If you've been following along, that will be:

```bash
cd
ls
cd src
ls
cd open-documentation-academy
```

## Build the documentation

At this point, you can build the documentation (as it currently exists) on your local machine, with:

```bash
make run
```

If `make run` doesn't work initially, then try running this sequence of commands to start with a clean build environment:

```bash
make clean
make install
make run
```

If it manages to complete the run successfully, you will see a big rush of commands and output flying past on your terminal window, and eventually, it will stop here:

![The documentation has built successfully](images/docs_being_served.png)

This means the documentation was successfully built, and now you can view it in your web browser by right clicking on that `http://127.0.0.1@8000` link and either selecting "open link" or "copy link" (which you can then paste into your browser of choice). 

It's really convenient to have this running while you're working on your changes, because every time you save a file, it will update the build and show you a live preview of what your changes look like. 

You can close the running server at any time by pressing `Ctrl` + `C` in the window where it's running.

It's a good idea to open a second Ubuntu tab in your Terminal Window so that you can work in one tab while the documentation can be served in the other. You can do this by clicking on the down arrow next to the currently open tab, and clicking "Ubuntu" (if you're using WSL). 


## DRAFT

```bash
multipass launch --cpus 2 --memory 4G --disk 15G --name sphinx

multipass list

multipass shell sphinx

python3 --version

pip --version

sudo apt install python3-pip python3.12-venv # I think this gets installed in the venv python3-sphinx

git clone https://github.com/canonical/sphinx-docs-starter-pack.git academy

# Note, I never use the `https` endpoint when cloning, as all of my GitHub repos require the `ssh` endpoint. But, in a Multipass machine I don't always copy my ssh keys over, and so I cannot use the `ssh` endpoint. If you are running this on the machine where you normally interact with GitHub, then use the `ssh` or `gh` API endpoint.

# git clone git@github.com:canonical/sphinx-docs-starter-pack academy
# gh repo clone canonical/sphinx-docs-starter-pack academy

cd academy

python3 -m venv .venv

make
```

Running `make` with no parameter lists the supported parameters:

```bash
 -------------------------------------------------------------
 * watch, build and serve the documentation:  make run
 * only build:                                make html
 * only serve:                                make serve
 * clean built doc files:                     make clean-doc
 * clean full environment:                    make clean
 * check links:                               make linkcheck
 * check spelling:                            make spelling
 * check spelling (without building again):   make spellcheck
 * check inclusive language:                  make woke
 * check accessibility:                       make pa11y
 * check style guide compliance:              make vale
 * check style guide compliance on target:    make vale TARGET=*
 * other possible targets:                    make <TAB twice>
 -------------------------------------------------------------
 ```

```bash
make html
```

Runnaing `make html` will initialize the environment before generating HTML output based on the `.rst` files included in the Canonical Sphinx starter pack. Selected parts of the output are shown here:

```bash
make -f Makefile.sp sp-html
make[1]: Entering directory '/home/ubuntu/academy'
python3 -m venv .sphinx/venv
. .sphinx/venv/bin/activate; python3 .sphinx/build_requirements.py
... setting up virtualenv
python3 -m venv .sphinx/venv

...

writing output... [100%] readme
generating indices... genindex done
writing additional pages... search done
dumping search index in English (code: en)... done
dumping object inventory... done
build succeeded.

The HTML pages are in _build.
make[1]: Leaving directory '/home/ubuntu/academy'
```

The first time `make html` is run the environment is set up. Some of the packages that are installed in the Python virtual environment (`venv`) are required by Sphinx, and some are optional packages specified in the Sphinx `conf.py`.

> Note:
>
> The Starter Pack supports serving the generated HTML with the `make run` command. This works fine if you are running Sphinx on your local machine, but if you are following this guide and running Sphinx in Multipass you should export some options for Sphinx (skip this if you are using remote desktop or X windows with Multipass):
>
> ```bash
> export SPHINXOPTS="--host 0.0.0.0 --port=8000"
> ```

```bash
make run
```


Get the IP Address of your Multipass instance
```bash
multipass list
```

The output provides you with the IP Address:

```
Name                    State             IPv4             Image
sphinx                  Running           192.168.77.3     Ubuntu 24.04 LTS
```

### Open a browser

Use the IP Address from the output of `multipass list` and the port number 8000. Using the above output as an example:

http://192.168.77.3:8000
