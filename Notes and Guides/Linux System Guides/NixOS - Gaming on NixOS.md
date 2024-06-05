---
sticker: lucide//gamepad-2
banner: https://i.imgur.com/lk40WVI.png
tags:
  - NIxOS
---
## NixOS Gaming setup  `fas:Steam`

**First thing we want to do is Enable OpenGL** 
- also going to enable it so that i can use wayland on my system. 

```nix
{ pkgs, ... }:

{
	hardware.opengl = {
		enable = true;
		driSupport = true;
		driSupport32Bit = true;
	};

	# For NVIDIA
	services.xserver.videoDrivers = ["nvidia"]; # For Nvidia
	hardware.nvidia.modesettings.enable = true;

    # For AMD
	services.xserver.videoDrivers = ["amdgpu"]; # If you have a AMD GPU
}
```

--------------------------------
## Hybrid Laptop and Optimus Prime `fas:LaptopCode`

*SYNC VS OFFLOAD*

- OFFLOAD mode, with offload mode the GPU will only be used when its needed. 
- SYNC mode, dedicated GPU runs at all time. 

#### SYNC `fas:Sync`
```nix
# Config.nix or Nvidia.nix
{ pkgs, ... }:
{
  hardware.nvidia.prime = {
	  sync.enabled = true;

      # Integrated
      amdgpuBusId = "PCI:0:0:0";

      nvidiaBusId = "PCI:0.0.0";
  };
}
```


#### OFFLOAD `ris:Loader4`
```nix
# Config.nix or Nvidia.nix
{ pkgs, ... }:
{
  hardware.nvidia.prime = {
    offload = {
      enable = true;
      enableOffloadCmd = true; #allows us to run the nvidia-offload command
    };
      # Integrated
      amdgpuBusId = "PCI:0:0:0";
      # dedicated
      nvidiaBusId = "PCI:0.0.0"
  };
}
```

To find out our PCI ID we can run the following command
```bash
nix shell nixpkgs#pciutils -c lspci | grep ' VGA '
```

![[Pasted image 20240605062909.png]]

> [!success]
> As you can see running this command shows us the PCI ID in my case its `nvidia:01:00.0` `AMD:06:00.0`

> [!warning]
> Please note if offload configuration was chosen we can use the `nvidia-offload` command to start a game or use <mark style='background:var(--mk-color-purple)'>hashcats</mark>
![[Pasted image 20240605064804.png|source - vimjoyer]]

what one to use? Well it depends on what you are doing and where you are, if you are connected to a power supply its best to use Sync but if you are travelling a lot and need to conserve battery, offload its better suited. 

This is NixOS however and you know we can do both. if we add these lines of code under the offload options. we can have it so that every time we boot we will see in our grub or boot loader that their will be 2 options offload and sync. 

![[Pasted image 20240605065219.png|Source - Vimjoyer]]

> [!NOTE]
> if you are lucky their might be your hardware configuration on the NixOS repo - [NixOS Hardware](https://github.com/NixOS/nixos-hardware)


## Setting Up Steam `fas:Steam`

Lets get started on setting up steam, to do this we need to go back into out configuration.nix once again and a few lines of code
```nix
# config.nix or steam.nix
{ pkgs, ... }:
{
  programs.steam.enable = true;   # Enableing Steam will also install it
  programs.steam.gamescopeSession.enable = true;    # Helps with upscaling or res

  # mangohud is a tool used to monitor game performance. 
  enviroment.systemPackages = with pkgs; [
    mangohud
  ];

  programs.gamemode.enable = true    #Better optimization for games, 
}
```

To make use of these tools we need to add them to our Launch options in steam. 
![[Pasted image 20240605070532.png|Source - Vimjoyer]]

# NOT FINISHED
https://youtu.be/qlfm3MEbqYA?t=338
## ToDo
- [ ] Install Proton and add it to the guide ➕ 2024-06-05
- [ ] Set up bottles and heroic {Steam Alternatives} ➕ 2024-06-05 ⏳ 2024-06-10
- [ ] add Sections about protondb website ➕ 2024-06-05 ⏳ 2024-06-10 📅 2024-06-10
