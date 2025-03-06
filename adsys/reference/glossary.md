# Glossary of terms

<!--
NOTE: Examples included for illustration
Pay particular attention to Ubuntu- or Linux-specific
concepts, tools or packages that a Windows users might
not be familiar with.
-->
:::{glossary}

Active Directory (AD)
  A directory service developed by Microsoft that provides centralized authentication, authorization, and management of users, computers, and resources in a networked environment.

Administrative Templates
  A set of policy settings that allow administrators to configure user and computer settings in a Windows-based Active Directory environment, often managed via Group Policy Objects (GPOs).

[adcli](https://manpages.ubuntu.com/manpages/xenial/man8/adcli.8.html)
  A command-line tool for managing Active Directory (AD) domain membership on Linux.

ADSys
  A tool that allows system administrators to manage Ubuntu machines using Microsoft Active Directory (AD).

[adsysctl](https://documentation.ubuntu.com/adsys/en/latest/reference/adsysctl-cli/)
  A command-line utility for interacting with the ADSys service in Ubuntu.

[Adwatchd](https://documentation.ubuntu.com/adsys/en/latest/reference/adwatchd/)
  A daemon that monitors and enforces compliance with Active Directory policies on Ubuntu systems, helping ensure settings are consistently applied.

apt (Advanced Package Tool)
  A package management system used in Debian-based distributions like Ubuntu to install, update, and remove software.

[AppArmor](https://documentation.ubuntu.com/server/how-to/security/apparmor/)
  A Linux security module that enforces mandatory access control policies on programs to limit their capabilities.

[certmonger](https://manpages.ubuntu.com/manpages/focal/man8/certmonger.8.html)
  A service that monitors and renews certificates, commonly used in enterprise environments.

Client
  In the context of ADSys, a client refers to an Ubuntu desktop or server that is managed using Microsoft Active Directory (AD).

D-Bus call
  A command or API request used to communicate with system services via D-Bus, a message bus system for interprocess communication.

Daemon
  A background process that runs continuously to provide system services.

[dconf](https://documentation.ubuntu.com/adsys/en/latest/explanation/dconf/)
  A low-level configuration system used by GNOME-based environments to store application and system settings, providing a centralized way to manage configurations.

Domain Controller (DC)
  A server in an Active Directory network that authenticates users, enforces security policies, and manages domain-wide resources.

FQDN (Fully Qualified Domain Name)
  A complete domain name that specifies the exact location of a device within the DNS hierarchy.

getcert
  A command-line tool used to request, monitor, and renew security certificates, often used with certmonger.

GNOME
  A popular open-source desktop environment for Linux systems, designed for ease of use and accessibility, providing a modern graphical user interface (GUI).

Group Policies
  A feature in Active Directory that allows administrators to define security settings, software installations, and user preferences across multiple computers in a domain.

GSettings
  A system for storing application and desktop settings in GNOME-based environments.

GVfs (GNOME Virtual File System)
  A user-space virtual filesystem that provides access to remote locations, such as FTP, SMB, and Google Drive.

Kerberos
  A network authentication protocol that uses tickets to securely authenticate users and services.

LDAP (Lightweight Directory Access Protocol)
  A protocol for accessing and managing directory information, commonly used for authentication.

Mount Managers
  Tools or processes that handle the mounting and unmounting of filesystems on Linux.

mounts
  The process of attaching a filesystem to a specific directory in the Linux file hierarchy.

PAM (Pluggable Authentication Modules)
  A framework for integrating various authentication methods into Linux systems.

[Polkit](https://manpages.ubuntu.com/manpages/focal/man8/polkit.8.html)
  A toolkit for defining and handling system-wide privileges in Linux.

proxy
  A server or service that acts as an intermediary for network requests, often used for filtering or caching.

realmd
  A service that allows automatic discovery and enrollment of Linux machines into Active Directory or other identity domains.

Samba
  A software suite that enables file and print sharing between Linux and Windows systems using the SMB/CIFS protocol.

Security Identifier (SID)
  A unique identifier assigned to users, groups, and other objects in Windows-based systems.

Server
  In the context of ADSys, the server refers to a **Windows Server** running Active Directory (AD), which manages and enforces policies for Ubuntu clients.

SSSD (System Security Services Daemon)
  A service that manages authentication and authorization with identity providers like Active Directory or LDAP. [SSSD is used with ADSys](https://documentation.ubuntu.com/adsys/en/stable/explanation/adsys-ref-arch/) for managing authentication and policies.

sudo
  A command that allows users to run programs with elevated (superuser) privileges.

systemd
  A modern system and service manager for Linux, responsible for initializing and managing system processes.

systemd journal
  A logging system that collects and organizes system logs for troubleshooting and auditing.

Ubiquity installer
  The default graphical installer for Ubuntu, designed to simplify OS installation.

visudo
  A command used to safely edit the sudoers file, which controls user permissions for executing commands with elevated privileges.

Ubuntu Pro
  A premium service from Canonical that provides extended security updates (ESM), compliance tools, and enterprise support for Ubuntu systems.

Winbind
  A component of Samba that allows Linux systems to authenticate users against a Windows domain. It can be used as an alternative to SSSD.
:::



