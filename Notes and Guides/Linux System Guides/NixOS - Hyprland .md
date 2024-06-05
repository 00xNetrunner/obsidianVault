
```avatar
image: Linux System Guides/Pasted image 20240605105715.png
description: |-
  Hyprland is one of the more Aesthetic windows managers. in this write up i am going to show you how to install and configure Hyprland in NIxOS. 

  I made this guide my following Vimjoyers video, i am making these guides for other people. and for myself as i have just started my NixOS Journey
  If you would like to follow Vimjoyers video it can be found here. - [Nixos and Hyprland - Best Match Ever](https://youtu.be/61wGzIv12Ds)
```

#### Getting started
The first thing we need to do is add this line to our <span style='color:var(--mk-color-purple)'>configuration.nix</span>: 
```nix
programs.hyprland.enable = true;
```

That is all you need to do if you are a AMD Users, for NVIDIA users however a more rigorous set up is required. add the lines of code to you <span style='color:var(--mk-color-purple)'>configuration.nix</span>
```nix
  # Enabling hyperland on NixOS
  programs.hyprland = {
    enable = true;
    #nvidiaPatches = true; ## NO LONGER NEEDED
    xwayland.enable = true;
  };

  environment.sessionVariables = {
    WLR_NO_HARDWARE_CURSORS = "1";
    NIXOS_OZONE_WL = "1";
  };

  hardware = {
    opengl.enable = true;
    nvidia.modesetting.enable = true;
  };
```

sudo 