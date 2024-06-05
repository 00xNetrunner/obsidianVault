
```avatar
image: Linux System Guides/Pasted image 20240605105715.png
description: |-
  Hyprland is one of the more Aesthetic windows managers. in this write up i am going to show you how to install and configure Hyprland in NIxOS. 

  I made this guide my following Vimjoyers video, i am making these guides for other people. and for myself as i have just started my NixOS Journey
  If you would like to follow Vimjoyers video it can be found here. - [Nixos,hyprland and Home-manager](https://youtu.be/zt3hgSBs11g)
```

#### Getting started
The first thing we need to do is add this line to our <span style='color:var(--mk-color-purple)'>configuration.nix</span>: 
```nix
{ pkgs, lib, inputs, ... }: {

  programs.hyprland.enable = true;
  programs.hyprland.package = inputs.hyprland.packages."${pkgs.system}".hyprland;
}
```

we should also add the hyprland.url to our flake.nix
```
{
  inputs = {
    hyprland.url = "github:hyprwm/Hyprland";
  };
}
```


Now thats done lets move on to our home-manager. and set up wayland config their. we can also add these settings to a hyprland.nix file that home-manager calls in. 
