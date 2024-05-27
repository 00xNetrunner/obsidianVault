---
banner: https://miro.medium.com/v2/resize:fit:1200/1*mTfxHVzOjAMPsBxHkWqbyQ.jpeg
sticker: lucide//hexagon
---
```avatar
image: Linux System Guides/NixOS_Images/Pasted image 20240526230808.png
description: NixOS is a Linux distribution that uses a declarative configuration model. It ensures system reproducibility, supports atomic upgrades and rollbacks, and manages packages through the Nix package manager. Configurations are defined in Nix files, enabling consistent and maintainable setups across different machines.
```


> [!important]
> Installing NIxOS is easy if you chose a GUI installer from the website, it uses the calamaris installer and is straight forward to use. allow unfree software when installing

## Installing Packages `fas:Codepen`

Installing packages on NixOS Is very unique as its decorative. meaning instead of using a command like `sudo apt install firefox`, we add it to the `configuration.nix` file. 

the configuration file holds all the system settings like language, date and time, location, etc

but it also where you declare what packages you want to install. 

![[Pasted image 20240526223736.png]]![[Pasted image 20240526223813.png]]

Because NixOS is Declarative, its easy to replicate on other system. as all you need is the config file. 

```bash
cd /etc/nixos
sudoedit configureation.nix # This Opens the config file in nano
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

> [!important]
> Add packages you want just by typing them under `environment.systemPackage` NIxOS has one of the largest number of available packages than any other system apart from arch. you can find all available packages here: [NixOS Packages](https://search.nixos.org/packages)
> 
> When you have finished adding you packages, you can update the system with this command:
> ```bash
> sudo nixos-rebuild switch
> ```

Their will be a lot of options you can also add to the config file. to learn about new modules
```bash
man configuration.nix
```

> [!tip]
> To search through the manual or find something specific use `/bluetooth`

After you have updated and reboot you will see 2 boot loader options, config 1 will be nix in its original state. while the config 2 will be the nix you just built.

![[Pasted image 20240527034130.png]]

> [!tip]
> If you want to build the new packages or update but not add the new option to the grub use the following command: 
> ```bash
> sudo nixos-rebuild test
> ```

NixOS will store every package in a directory called `/nix/store`
![[Pasted image 20240527034437.png]]
> [!tip]
> To keep your system clean:
> ```bash
> sudo nix-collect-garbage --delete-older-than 15d
> ```

## Flakes

> [!summary]
> When we rebuild our nixos system  it does not actuality update the packages, all nixos packages can be found in the nixpkgs GitHub.  its split into different branches. the main ones are 
> - stable (23.11) 
> - unstable
> - upstream
> 
> When you install nixos by default you will be on the stable branch. all packages you install and nixos options you apply  are taken from it. this commit is called a channel and it stays the same until you explicitly update to a newer commit. 

> [!warning]
> This is going to be a problem when you try and share you configuration files to someone as they may not be on the same commit as you.  they could be on an unstable branch or may be on an older commit. 

> [!help]
> This is where nix flakes come in. Nix Flakes are a special system for managing you Nix code dependencies in a declarative way. this is why we will create a flake that explicitly lists your nix packages branch as a dependency, the Flake system will then automatically create a lock file to keep track of its updates. this makes it so that our configurations are not dependent on our systems but can be applied on anyone's system no matter what commit they are on. 

Ensure that you have Nix installed with Flakes support enabled. You can enable Flakes support by adding the following lines to your `/etc/nixos/configuration.nix`:
```bash
nix.settings.experimental-features = ["nix-command" "flakes" ];
```

Now from rebuild your system again with 
```bash
sudo nixos-rebuild switch
```

from here make sure you are in the `/etc/nixos` directory and run 
```bash
sudo nix flake init --template github:vimjoyer/flake-starter-config
```
![[Pasted image 20240527045344.png]]

when you open up this flake 
![[Pasted image 20240527045518.png]]
>extraSpecialArgs will not work nix uses SpecialArgs now

Now that we have made our flake we can rebuild are system with said flake with the following command. 
```bash
sudo nixos-rebuild switch --flake /etc/nixos#default
```
> now use `ls` command you will see a new file called flake.lock

we are now going to add a popular nixos module called home-manager. but first we should probably talk about what they are. 

![[Pasted image 20240527050039.png]]
Modules in nix are chunks of nix code that extend your configuration by setting options or providing new ones they will normally look something like this. they are basically functions that take some arguments and return dictionaries with options

if it looks familiar this is because the config.nix and hardware config are both Nix Modules. 

making your own module is simple,  Create a new nix file: 
![[Pasted image 20240527051106.png]]
>main-user.nix 
>this module defines my systems primary user and sets its shell to zsh

you can then import this new module in the configuration.nix like so:
![[Pasted image 20240527051224.png]]

##### NOT FINISHED CLICK LINK BELOW TO WATCH THE VIDEO AND FINISH THE GUIDE
[Ultimate NixOS Guide | Flakes | Home-manager @ 9:48](https://youtu.be/a67Sv4Mbxmc?t=588)

