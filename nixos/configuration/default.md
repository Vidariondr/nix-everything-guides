# Table of contents

- [Core information](#core-information)
  - [Syntax basics](#syntax-basics)
    - [Attribute sets](#attribute-sets)
    - [Importing files](#importing-files)
    - [Lists](#lists)
    - [with syntax](#with-syntax)
  - [Adding packages](#adding-packages)
  - [The nixos-rebuild command](#the-nixos-rebuild-command)
  - [Getting options and packages](#getting-options-and-packages)
- [Navigation](#navigation)

# Core information

The configuration of your NixOS system is stored in
`/etc/nixos/configuration.nix` and `/etc/nixos/hardware-configuration.nix`
files. Both of them are written in the Nix language.

If you want to track your configuration with Git, you have to do this:

```
# Move the configuration files to you git repo location
mv /etc/nixos /home/xyz/git-repo-dir

# Change the file ownership of the files, since right now it's root
chown -R xyz:xyz /home/xyz/git-repo-dir/nixos

# Create a symlink
ln -s /home/xyz/git-repo-dir/nixos /etc/nixos
```

You have to do this, because `nixos-rebuild` command, that you use for applying
the configuration, uses the `/etc/nixos/configuration.nix` file.
`/etc/nixos/hardware-configuration.nix` is imported in the
`/etc/nixos/configuration.nix` file.

Remember to stage all the imported files. Nix won't see the files if they aren't
in the git tree, which may cause errors.

## Syntax basics

Let's explore the initial configuration file to understand how everything works.

Each Nix configuration file is basically a function that is executed by the
`nixos-rebuild` command. In Nix, `:` means you're dealing with a function:

```
x: x + 1
```

The left side is the function argument, while the right is function body.

If you open the `configuration.nix` file, you will find the
`{ config, pkgs, ...}:` line. You see the `:` which means it's a function, and
to the left is the function argument.

### Attribute sets

`{}` in nix is an attribute set. It's a way to pass multiple values to the
function. Think of it as JSON key-value pairs. But you probably noticed there is
only one word in the `configuration.nix` file - `config` or `pkgs`. Here, it's
deconstructing. The function expects an attribute set, and you're telling it to
extract the `config` and `pkgs` values and assign them to `config` and `pkgs`.
`...` tells it to ignore any other values that are passed to the function.

You will find attribute sets all over the configuration file. In fact, the whole
function body is an attribute set. Each key-value pair should have a `;` at the
end.

### Importing files

If you want, you can split the configuration into multiple files and then import
them. Just like the `hardware-configuration.nix` is imported in the
`confiugration.nix`. You'll notice that the hardware file's path is between
`[]`.

The main file is `configuration.nix`, so import them in that file.

### Lists

In Nix, lists use `[]`. Items are separated with white space.

### `with` syntax

You'll find the `with zzz;` syntax in a few places in the configuration file.
It's a shorthand. These are equivalent:

```
with zzz; [
xyz
zyx
]
```

```
[
zzz.xyz
zzz.zyx
]
```

### `system.stateVersion`

DO NOT CHANGE THAT UNLESS YOU KNOW WHAT YOU'RE DOING. EVEN THEN A FRESH INSTALL
IS PREFERRED.

This option defines the first NixOS version you installed on your system. It's
used for maintaining compatibility with some packages.

# Adding packages

To add packages you can either add them to the `environment.systemPackages` or
`user.users.xyz.packages`.

`environment.systemPackages` installs packages for all users.
`user.users.xyz.packages` installs packages only for that user.

# The `nixos-rebuild` command

Now that you have the configuration the way you want it, you can activate it
with the `nixos-rebuild` command.

- `nixos-rebuild switch` - this will build and activate the new configuration,
  and make it the boot default.
- `nixos-rebuild boot` - this will build the new configuration, and make it the
  boot default. It will not be activated.
- `nixos-rebuild test` - this will build and activate the new configuration. If
  you reboot the system, it will boot with the previous configuration.

# Getting options and packages

My recommendation is [MyNixOS](https://mynixos.com/).

# Navigation

- [NixOS Overview](/nixos/README.md)
- [Installation Overview](/nixos/installation/README.md)
- [Configuration Overview](/nixos/configuration/README.md)
