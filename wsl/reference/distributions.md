# Distributions

Our flagship distribution (distro) is Ubuntu. It is the default option when you install WSL for the first time. However, we develop several flavours for the Ubuntu distro and one or more of these flavours may fit your needs better.

Each of these flavours corresponds to a different application on the Microsoft Store, and once installed, they'll create different flavors in your WSL. 

These are the variants of the default Ubuntu flavor we develop and maintain:

- [Ubuntu](https://apps.microsoft.com/detail/9PDXGNCFSCZV?hl=en-us&gl=US) ships the latest stable LTS release of Ubuntu. When new LTS versions are released, Ubuntu can be upgraded once the first point release is available.
- Numbered releases, for example [Ubuntu 22.04.3 LTS](https://apps.microsoft.com/detail/9PN20MSR04DW?hl=en-us&gl=US) refer to specific Long Term Stability (LTS) releases that are supported for five years. For more information on releases, support and timelines, visit the [Ubuntu releases page](https://wiki.ubuntu.com/Releases).
- [Ubuntu (Preview)](https://apps.microsoft.com/detail/9P7BDVKVNXZ6?hl=en-us&gl=US) is a daily build of the latest development version of Ubuntu previewing new features as they are developed. It does not receive the same level of QA as stable releases and should not be used for production workloads.

## Naming
The variants of the default Ubuntu flavor is available under different names depending the context you try to access it with. Here is a table matching them.

1. App name is the name you'll see in the Microsoft Store and Winget.
2. AppxPackage is the name you'll see in `Get-AppxPackage`.
3. Distro name is the name you'll see when doing `wsl -l -v` or `wsl -l --online`.
4. Executable is the program you need to run to start the distro.

| App name             | AppxPackage name                       | Variant      | Executable          |
| -------------------- | -------------------------------------- | ---------------- | ------------------- |
| `Ubuntu`             | `CanonicalGroupLimited.Ubuntu`         | `Ubuntu`         | `ubuntu.exe`        |
| `Ubuntu (Preview)`   | `CanonicalGroupLimited.UbuntuPreview`  | `Ubuntu-Preview` | `ubuntupreview.exe` |
| `Ubuntu XX.YY.Z LTS` | `CanonicalGroupLimited.UbuntuXX.YYLTS` | `Ubuntu-XX.YY`   | `ubuntuXXYY.exe`    |
