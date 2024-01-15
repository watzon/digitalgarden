---
{"dg-publish":true,"permalink":"/notes/docker-with-nvidia-in-nix-os/","tags":["nix","nixos","nvidia","docker"],"noteIcon":""}
---

Enabling Nvidia support for Docker in NixOS is easy, but there is a step missing in most instructions that will leave you scratching your head.

First, add the following to `configuration.nix`:

```nix
virtualization.docker = {
  enable = true;
  enableNvidia = true;
};
```

and then run `sudo nixos-rebuild switch`. If all goes well, great, but I got an error my first time:

```
error:
Failed assertions:
- Option enableNvidia on x86_64 requires 32bit support libraries
```

Turns out, direct rendering for 32 bit applications isn't enabled on NixOS by default. The fix is simple. Open `hardware-configuration.nix` and add the following line:

```nix
hardware.opengl.driSupport32Bit = true;
```

and run `sudo nixos-rebuild switch` again. Everything should work as expected this time.