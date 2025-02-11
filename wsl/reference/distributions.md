# Distributions

Our flagship distribution (distro) is Ubuntu. It is the default option when you install WSL for the first time. Several releases of the Ubuntu distro are available for WSL.


Each release of Ubuntu for WSL is available as an application from the Microsoft Store. Once a release is [installed](https://documentation.ubuntu.com/wsl/en/latest/howto/install-ubuntu-wsl2/#method-1-microsoft-store-application), it will be available to use in your WSL environment. 

## Releases of Ubuntu on WSL
These are the releases of Ubuntu that we support and that are available on the Microsoft Store:

- [Ubuntu](https://apps.microsoft.com/detail/9PDXGNCFSCZV?hl=en-us&gl=US) ships the latest stable LTS (Long Term Support) release of Ubuntu. When new LTS versions are released, this release of Ubuntu can be upgraded once the first point release is available.
- Numbered releases, for example [Ubuntu 22.04 LTS](https://apps.microsoft.com/detail/9PN20MSR04DW?hl=en-us&gl=US), refer to specific Long Term Stability (LTS) releases that receive standard support for five years. For more information on LTS releases, support and timelines, visit the [Ubuntu releases page](https://wiki.ubuntu.com/Releases). Numbered releases of Ubuntu on WSL will not be upgraded unless configured to upgrade in `etc/update-manager/release-upgrades`.
- [Ubuntu (Preview)](https://apps.microsoft.com/detail/9P7BDVKVNXZ6?hl=en-us&gl=US) is a daily build of the latest development version of Ubuntu, which previews new features as they are developed. It does not receive the same level of QA as stable releases and should not be used for production workloads.

:::{note}
Interim releases of Ubuntu are currently not supported on WSL.
:::

## Naming
Depending on context, releases of Ubuntu are referred to by different names: 

1. **App name** is the name of the application for a specific Ubuntu release that you will see in the Microsoft Store.  
2. **AppxPackage** is the name that can be passed to `Get-AppxPackage -Name` in PowerShell to get information about an installed package. 
3. **Distro name** is the name logged when you invoke `wsl -l -v` to list installed releases of Ubuntu.  
4. **Executable** is the program you need to run to start the Ubuntu distro.

These naming conventions are summarised in the table below:

| App name             | AppxPackage name                       | Distro name      | Executable          |
| -------------------- | -------------------------------------- | ---------------- | ------------------- |
| `Ubuntu`             | `CanonicalGroupLimited.Ubuntu`         | `Ubuntu`         | `ubuntu.exe`        |
| `Ubuntu (Preview)`   | `CanonicalGroupLimited.UbuntuPreview`  | `Ubuntu-Preview` | `ubuntupreview.exe` |
| `Ubuntu XX.YY.Z LTS` | `CanonicalGroupLimited.UbuntuXX.YYLTS` | `Ubuntu-XX.YY`   | `ubuntuXXYY.exe`    |

:::{note}
The kernel used in WSL environments is maintained by Microsoft.
Bug reports and support requests for the WSL kernel should be directed to the [official repository for the WSL kernel](https://github.com/microsoft/WSL2-Linux-Kernel).
:::