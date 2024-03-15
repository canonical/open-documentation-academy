# Install git

To work with GitHub via the command line, we want to install `git` and set it up. Type the following command into your Ubuntu terminal window, and press enter to run it:

```bash
sudo apt install git
```

If you ever need to check what version of git you have (or if it's already installed) you can use the following command:

```bash
git --version
```

## Configure git

Now `git` is installed, we need to configure it so that GitHub can link up with your account.

Add your GitHub username (the one you use to log in to your GitHub account) by running the following command. Remember to substitute `your_username` for your actual GitHub username.

```bash
git config --global user.name your_username
```

Now let's do the same with your email (substitute for your GitHub email):

```bash
git config --global user.email your.email@canonical.com
```

This next command isn't strictly required, but is recommended to replace "master" (the default branch name) with "main" which is the more inclusive standard that many organisations are moving to. You don't need to change anything about this command, you can copy and paste it directly into your terminal window and press enter to run it:

```bash
git config --global init.defaultBranch main
```

If you want to double check the options you've configured so far, you can type:

```bash
git config --list
```

Which will show all of the configuration options that have been set.

### Install the GitHub Command Line Interface (CLI)

This will make your life much easier! On the GitHub website you'll often see commands that start with `gh`. These commands can usually be run as-is without you needing to know the corresponding sequence of `git` commands if you have the GitHub CLI. Let's install it and authorise it to access GitHub. 

To install, use the following command in your Ubuntu terminal window:

```bash
sudo apt install gh
```

### Authorise GitHub CLI to access your GitHub account

We now want to log into the authorisation helper with the following command:

```
gh auth login
gh auth setup-git
```

This will give you a series of options directly in the terminal that you can choose from, and once we go through them, it will connect your GitHub account to your Ubuntu terminal.

### Configure the GitHub CLI

You can refer to the following screenshot for help. When prompted with each of these questions, choose the option highlighted in blue. These are the simplest settings for authenticating your account. You can choose different options if you know how to set up SSH, for example, but for the easiest possible setup, these are the best options.

![gh configuration options](images/gh_configuration.png)

At the end of this step, `gh` will give you an alpha-numeric code in the format `XXXX-XXXX` directly in your terminal window. Copy this code, because you'll need it for the next step.

`gh` will then try to authenticate using your browser. It will open up a new tab or window in your internet browser and ask you to copy and paste "the code" into the spaces provided. This step will connect `gh` in your Ubuntu terminal to GitHub on the web.

### Authenticate your GitHub login

GitHub will then, in your browser, ask you to re-log in and authenticate using 2-factor authentication to confirm that it's actually you who requested the `gh` authentication above. You will usually be sent a second code (this time via your phone or other 2FA device), and it will then complete the login and connect everything up. You'll get a confirmation in your Ubuntu terminal windows and lots of green ticks everywhere if everything went to plan!

## Clone the repository onto your machine

Now we can clone repositories directly to your machine using the HTTPS option. If you intend to do work on multiple repositories, it's a good idea to first make a folder to put them all in. I have called my folder `src` - you can call it something else if you'd like, or if you're happy with `src` you can copy and paste this command directly into your terminal:

```bash
mkdir src
cd src
```

After making the folder (with `mkdir`), we have then navigated to it using `cd` (change directory).

Now that we are inside the new folder, we need to find the "address" for the repository we want to clone. In our browser window, let's go to the GitHub website and navigate to the repository we're interested in cloning. In this case, I'm using the [Open Documentation Academy](https://github.com/canonical/open-documentation-academy) repository, but you can use another repository if you like.

Now, we can click on the green button that says `< > Code`, click on the "Local" tab, and then on the HTTPS sub-tab. Copy the URL that's shown in the box below that. 

![clone the repository](clone_repo.png)

We can then return to our terminal window and clone the repository - the command always looks something like this:

```bash
git clone <repository URL or SSH address>
```

In this case, we want to use the HTTPS link, so run this command to clone the Open Documentation Academy repository:

```bash
https://github.com/canonical/open-documentation-academy
```

This will download everything into a new folder *inside your `src` folder*, called "open-documentation-academy". This will be important later!





