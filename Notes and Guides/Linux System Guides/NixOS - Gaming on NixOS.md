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

