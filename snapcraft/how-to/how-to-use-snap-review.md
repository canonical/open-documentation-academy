# How to use the snap review-tools to verify your snap

`review-tools` is a tool that checks whether a snap meets the requirements for the Snap Store. Once you have created a snap it is highly recommended to use `review-tools` to verify it before submitting it to the Store. 

This guide shows how to use `review-tools`, and considers some of its commands.

## Prerequisites

This guide assumes:
- Command line knowledge
- A Linux system running Ubuntu
- An already built snap

## Enable snapd

First check whether `snapd` is enabled on your system. If you are using Ubuntu 16.04 LTS (Xenial Xerus) or later you most likely have `snapd` installed. 

In the terminal type:

```bash
snap version
```

If you have `snapd` you will get something that looks like:

```no-highlight
snap    2.63+22.04
snapd   2.63+22.04
series  16
ubuntu  22.04
kernel  6.5.0-41-generic
```

If you do not have _snap_ it can be installed in two ways:

1. Search for _snapd_ on the Ubuntu Software Centre and install
2. Using the command line:

```bash
sudo update
sudo apt install snapd
```

Then either log out and back in again, or restart your system. 

## Install review-tools

Once you have ensured you have snap, you need to install  `review-tools`, which contains the necessary tools to review your snap. From the command line  run:

```bash
sudo snap install review-tools
```

## Using review-tools

Let us suppose your snap is named `awesomeApp.snap` and is located in the home folder. 

Type: 

```
review-tools.snap-review awesomeApp.snap 

```

If there are no problems with your snap, the command will return something like:

```
awesomeApp.snap: pass
```

If there are any issues, the output will list them. These issues can range from missing metadata to security concerns. 

## Common Issues

Based on the output you will need to address the reported issues. Below are some common problems and how to fix them. 

### Missing Metadata

If you see warnings about missing fields like `description` or `license`, you need to add these fields to your `snapcraft.yaml` file. 

### Security Warnings

Security warnings might indicate that your snap is using restricted interfaces without proper permissions.

## Retest the Snap

After making the necessary changes rebuild your snap and rerun the review tools to ensure all issues have been resolved. 

```bash
review-tools.snap-review awesomeApp.snap
```

If your snap passes you can then submit it to the Snap Store.

## Conclusion

Using `review-tools` is a key step in ensuring your snap package is secure, consistent, and ready for publication in the Snap Store. By following this guide, you can identify and fix common issues before submitting your snap, increasing the likelihood of a smooth approval process. How to use the snap review-tools to verify your snap.  