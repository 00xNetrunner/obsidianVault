---
banner: https://miro.medium.com/v2/resize:fit:1200/1*mTfxHVzOjAMPsBxHkWqbyQ.jpeg
sticker: lucide//hexagon
---
```avatar
image: Linux System Guides/NixOS_Images/Pasted image 20240526230808.png
description: NixOS is a Linux distribution that uses a declarative configuration model. It ensures system reproducibility, supports atomic upgrades and rollbacks, and manages packages through the Nix package manager. Configurations are defined in Nix files, enabling consistent and maintainable setups across different machines.
```

---

## **NixOS Installation Guide** `fas:Download`

> [!IMPORTANT]
> Installing NixOS is straightforward if you choose a GUI installer from the website. It uses the Calamares installer and is easy to navigate. Ensure you allow unfree software during installation.

## Table of Contents `fas:ListOl`

```table-of-contents
```

## **Installing Packages** `fas:Box`

Installing packages on NixOS is unique due to its declarative nature. Instead of using a command like `sudo apt install firefox`, you add the package to the `configuration.nix` file. This configuration file holds all system settings, including language, date and time, location, and installed packages.

![[Pasted image 20240526223736.png]]
![[Pasted image 20240526223813.png]]

Because NixOS is declarative, it is easy to replicate on other systems—all you need is the configuration file.

```bash
cd /etc/nixos
sudoedit configuration.nix  # This opens the config file in nano
```

```nix
{ config, pkgs, ... }:

{
  imports =
    [ ./hardware-configuration.nix ];

  boot.loader.grub.device = "/dev/sda";

  networking.hostName = "my-nixos";

  time.timeZone = "America/New_York";

  environment.systemPackages = with pkgs; [
    vim
    git
    firefox
  ];

  services.sshd.enable = true;

  users.users.yourusername = {
    isNormalUser = true;
    extraGroups = [ "wheel" ];
    password = "hashed-password";
  };
}
```

> [!IMPORTANT]
> To add packages, simply type them under `environment.systemPackages`. NixOS has one of the largest collections of available packages, second only to Arch. You can find all available packages here: [NixOS Packages](https://search.nixos.org/packages).
> 
> After adding your packages, update the system with:
> ```bash
> sudo nixos-rebuild switch
> ```

There are many additional options you can include in the config file. To learn about new modules, use:
```bash
man configuration.nix
```

> [!TIP]
> To search through the manual for specific topics, use `/search_term`.

After updating and rebooting, you will see two boot loader options: the original NixOS configuration and the newly built configuration.

![[Pasted image 20240527034130.png]]

> [!TIP]
> To build new packages or update without adding a new boot option to GRUB, use:
> ```bash
> sudo nixos-rebuild test
> ```

NixOS stores every package in a directory called `/nix/store`.

![[Pasted image 20240527034437.png]]

> [!TIP]
> To keep your system clean, run:
> ```bash
> sudo nix-collect-garbage --delete-older-than 15d
> ```

## Understanding Flakes `fas:Snowflake`

> [!SUMMARY]
> Rebuilding the NixOS system does not actually update the packages. All NixOS packages are found in the `nixpkgs` GitHub repository, divided into different branches such as:
> - stable (23.11)
> - unstable
> - upstream
> 
> By default, NixOS is installed on the stable branch. The packages you install and the options you apply are from this branch. This commit is called a channel and remains the same until you explicitly update to a newer commit.

> [!WARNING]
> Sharing configuration files can be problematic if others are not on the same commit as you. They could be on an unstable branch or an older commit.

> [!HELP]
> This is where Nix Flakes come in. Nix Flakes manage Nix code dependencies declaratively. By creating a flake that lists your Nix packages branch as a dependency, the Flake system automatically creates a lock file to track updates. This ensures configurations are not system-dependent and can be applied on any system regardless of the commit.

Ensure you have Nix installed with Flakes support enabled. Add the following lines to your `/etc/nixos/configuration.nix`:
```nix
nix.settings.experimental-features = ["nix-command" "flakes"];
```

Rebuild your system:
```bash
sudo nixos-rebuild switch
```

Navigate to the `/etc/nixos` directory and initialize a flake with:
```bash
sudo nix flake init --template github:vimjoyer/flake-starter-config
```

![[Pasted image 20240527045344.png]]

Open the flake file:
![[Pasted image 20240527045518.png]]

> Note: `extraSpecialArgs` will not work; Nix uses `SpecialArgs` now.

Rebuild the system with the flake:
```bash
sudo nixos-rebuild switch --flake /etc/nixos#default
```

> After running the command, use `ls` to see the new `flake.lock` file.

We will now add a popular NixOS module called home-manager. First, let's understand what modules are.

![[Pasted image 20240527050039.png]]

Modules in Nix are chunks of code that extend your configuration by setting or providing new options. They typically look like functions that take some arguments and return dictionaries with options. The `configuration.nix` and `hardware-configuration.nix` files are examples of Nix modules.

Creating your own module is simple. Create a new Nix file:
![[Pasted image 20240527051106.png]]

For example, `main-user.nix`:
> This module defines my system's primary user and sets its shell to zsh.

You can import this new module in the `configuration.nix` file:
![[Pasted image 20240527051224.png]]

>lets add some more options to the module we are creating.  the first option will be used to toggle our module. `main-user.enable` and the second module `main-user.userName` will determine our users name.  the file should look like this after
![[Pasted image 20240527174348.png]]

>These options can be used in another module now so lets call them into our main configuration.nix file.  here we can set the main user to be enabled, and set their username to yurii
![[Pasted image 20240527174653.png]]

> Cleaning up our code to make it look neater. 
![[Pasted image 20240527174938.png]]

Certainly! Here's the academically styled guide with callouts and icons:

---

## **Home-Manager** `fas:UserCog`

Now that we have explained what flakes are, let us download and configure Home-Manager. In the `flake.nix` file that we created, we will add the Home-Manager module.

```nix
home-manager = {
  url= "github:nix-community/home-manager";
  inputs.nixpkgs.follows = "nixpkgs";
}
```

We will also add `inputs.home-manager.nixosModules.default` to our configuration. The updated `flake.nix` file will look like this:

![[Pasted image 20240527180440.png]]

Next, we need to add `inputs.home-manager.nixosModules.default` to the `configuration.nix` file as well:

![[Pasted image 20240527180524.png]]

> [!QUESTION]
> We have added Home-Manager to NixOS, but what exactly is it?

The Home-Manager allows us to declaratively configure the home directory and manage configuration files stored there. Instead of configuring systems and services individually (e.g., editing Alacritty's TOML file), Home-Manager lets us manage configurations universally.

![[Pasted image 20240527181201.png]]

Let us modify the `configuration.nix` file as follows:

![[Pasted image 20240527181317.png]]

> Remember to change your username.

We also need to generate the `home.nix` file with the following command:

```bash
nix run home-manager/master -- init && \
  sudo cp ~/.config/home-manager/home.nix /etc/nixos/
```

> [!TIP]
> To view the manual for `home.nix`, use the following command:
> ```bash
> man home-configuration.nix
> ```
> Similar to other manuals, you can use `/` to search within the manual, e.g., `/programs.nix`.

Within the `home-config.nix` file, we can configure various settings such as themes. Home-Manager provides extensive capabilities, but this is a simple example.

![[Pasted image 20240527182102.png]]

### **Structuring The NixOS Configuration** `fas:FolderOpen`

Currently, the `/etc/nixos` directory is somewhat disorganized:

![[Pasted image 20240527182238.png]]

To improve organization, create two new folders in this directory using `mkdir -p`:

```bash
mkdir -p hosts/default
```

Move `configuration.nix`, `hardware-config.nix`, and `home.nix` into the `hosts/default` folder.

Ensure you update the `flake.nix` file with the new location for `config.nix`:

![[Pasted image 20240527182558.png]]

This setup allows for multiple configurations, such as a default configuration and a work-machine configuration, enabling easy switching between them:

![[Pasted image 20240527182815.png]]

To use this setup, add it to the `flake.nix` file:

![[Pasted image 20240527182851.png]]

To switch between configurations or set up one system on another, use the following commands:

Rebuild NixOS with the default configuration:

```bash
sudo nixos-rebuild switch --flake /etc/nixos#default
```

Rebuild NixOS with the work-machine configuration:

```bash
sudo nixos-rebuild switch --flake /etc/nixos#workmachine
```

In `/etc/nixos`, create the necessary folders:

![[Pasted image 20240527183208.png]]

This practice of splitting our modules into smaller files makes it easier to manage and select specific configurations as needed:

![[Pasted image 20240527183334.png]]

## Adding Unstable Packages. 
Mentioned above we talked about how their are 3 different channels, Nix Packages are some of the most bleeding edge. the nix repository has even more packages than the AUR especially the Nix Unstable branch, 

Lets say you want the newest neovim, well to do so you will need to add NixOS unstable branch to your system. to do this first lets input the following command

```bash
nix-channel -add \
https://nixos.org/channels/nixos-unstable nixos
nixos-rebuild switch --upgrade
```

![[Pasted image 20240528122508.png]]

## Creating a rebuild script
As a NixOS user you will be rebuilding and editing a lot of configs, its considered good practice to create a script to automate this process, for example here is a script created by YouTuber **No Boilerplate** 

![[Pasted image 20240528123418.png]]

A quick breakdown of what is happening here is the script opens up his .nix config file, the user can add what they want. soon as they save and exit, adds it to their git repo for nix, then rebuilds the system. when the system has rebuild it gets saved to a log, then the cat command is used to show if their has been any errors. 

this is just to be used as an example, its best to create your own script tailored to you. 

![[Pasted image 20240528124604.png]]







---

**Reference List:**

Vimjoyer. 2024. Ultimate NixOS Guide | Flakes | Home-manager. [online video] Available at: [https://youtu.be/a67Sv4Mbxmc](https://youtu.be/a67Sv4Mbxmc) [Accessed 27 May 2024].

Vimjoyer. 2024. flake-starter-config. [online] Available at: [https://github.com/vimjoyer/flake-starter-config](https://github.com/vimjoyer/flake-starter-config) [Accessed 27 May 2024].