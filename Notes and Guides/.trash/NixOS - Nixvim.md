---
banner: 
---
#NIxOS 

The easiest way to get NixVim up and running is via flake. the following command will download the flake from the nix-community GitHub repo:~
```bash
nix flake init --template github:nix-communtiy/nixvim 
```

to run nixvim just enter `nix run`

at first the nixvim will be very plain, but we can change this in the config/default.nix

config file, first lets set the color scheme: 

![[nixvim.png]]
> If you now run nix run again you will see that we have a new colorscheme

Lets add in some plugins

![[GOe0C7Qtypk 00-06-45 (2) Nixvim Configure Neovim with the power of Nix (NeovimConf 2023).png]]
