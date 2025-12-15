# zed-cached

Pre-built Zed editor binaries for NixOS, cached via Cachix.

The official repo only builds and caches a debug build. This fixes that.

## Usage

Add the Cachix cache to your Nix configuration:

```nix
nix.settings = {
  substituters = [ "https://zed-cached.cachix.org" ];
  trusted-public-keys = [ "zed-cached.cachix.org-1:+5QWh/HAC6AidVzXAKSR1LklqCAxSZrlLwanT86yEzo=" ];
};
```

Then use this flake:

```nix
# flake.nix
{
  inputs.zed-cached.url = "github:jcdickinson/zed-cached/stable";

  outputs = { self, zed-cached, ... }: {
    # Use zed-cached.packages.x86_64-linux.default
  };
}
```

Or run directly:

```sh
nix run github:jcdickinson/zed-cached/stable
```

## How it works

A GitHub Action runs every 6 hours to check for new stable Zed releases. When a new release is found:

1. Creates a branch `zed-{version}`
2. Updates `flake.lock` to pin to that release
3. Builds the package for `x86_64-linux`
4. Pushes the build to Cachix
5. Updates the `stable` branch

The `stable` branch always points to the latest stable Zed release with pre-built binaries available in Cachix.
