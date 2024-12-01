## How Nix works?

1. Multiple versions
2. Complete dependencies (dependencies declared explicitly)
3. Multi-user support
4. Atomic upgrades and rollbacks
5. Garbage collector `nix-collect-garbage` 
6. Functional package language (packages are built by **nix expressions**

## How NixOS works

NixOS is built on top of Nix package manager which uses Nix (pure functional language)
Packages are never overwritten after they have been built
There is no `/bin, /sbin, /lib, /usr` instead all packages are stored in `/nix/store/`

1. Declarative system configuration model `/etc/nixos/configuration.nix`
2. Reliable upgrades (`nixos-rebuild switch` will produce same results)
3. Atomic upgrades
4. Rollbacks (`nixos-rebuild switch --rollback`)
5. Reproducible system environments (copy configuration.nix to target system)
6. Safe to test changes (`nixos-build test`)