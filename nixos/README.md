# NixOS Overview

## What is NixOS?

NixOS is a Linux distribution based on the Nix package manager. The main
"selling point" of it is that the configuration is declarative - instead of
using commands to install packages, or change system settings, here you set
(nearly) everything in a configuration file. This allows you to back
up/version/share the configuration. It makes reinstalling the system pretty
easy, too, if you need to. NixOS can't declaratively configure the settings in
the `/home` directory, but we have the
[homemanager](https://nix-community.github.io/home-manager/) project for that.

## Features

- declarative, reproducible configuration - put the configuration files on
  another machine and it will create the same system.
- worry-free updates - NixOS doesn't update packages. Instead, the whole system
  is rebuilt as a new generation. If the new generation doesn't work for you,
  you can roll back to the previous one, or a one from a week ago if you want,
  without hassle. Just pick a different one from the boot manager.
- each package is build with its own version of dependencies - no dependency
  hell.
- mulitple versions of the same package can be installed.
- module system - you can have your entire GNOME configuration set behind a
  true/false switch. _GNOME = true_
- the [nixpkgs](https://github.com/NixOS/nixpkgs) repository. 120k packages and
  counting.

## Why use NixOS?

I've used Arch (btw) for a while, before switching to NixOS, and at some point I
started worrying that my system would break. This would mean setting everything
up all over again (well at least the system settings, dotfiles exist after all).
So before the inevitable disaster happened, I've decided to switch.

If you ever feel like this, NixOS might be a good choice. IT IS a completely
different approach to what you know from other Linux distributions, and IT IS a
bit difficult to configure at first, but the benefits are well worth it. At
least worth a try.

# Chapters

- [Installation](installation/README.md)
- [Configuration](configuration/README.md)
