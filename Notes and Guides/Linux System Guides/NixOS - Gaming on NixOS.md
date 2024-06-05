---
sticker: lucide//gamepad-2
banner: https://i.imgur.com/lk40WVI.png
---
## NixOS Gaming setup 

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

	services.xserver.videoDrivers = ["nvidia"]; # For Nvidia
	services.xserver.videoDrivers = ["amdgpu"]; # If you have a AMD GPU
}
```

