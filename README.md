# pebble.nix

Tools for building Pebble apps on Nix systems

## Quickstart

Add the following to a `shell.nix` file, then use `nix-shell`:

```nix
(import
  (builtins.fetchTarball https://github.com/Sorixelle/pebble.nix/archives/master.tar.gz)
).shellNix.default { }
```

Or, if using Nix flakes + `nix develop`:

```nix
{
  inputs.pebble.url = github:Sorixelle/pebble.nix;

  outputs = { self, pebble }: {
    devShell.${system} = pebble.devShell.${system} { };
  };
}
```

### Pinning

Using `builtins.fetchTarball` only caches a tarball for an hour by default. If
you don't want to have to redownload pebble.nix every hour, you can pin to a
specific commit.

```nix
builtins.fetchTarball {
  # Get the desired commit hash at https://github.com/Sorixelle/pebble.nix/commits
  url = https://github.com/Sorixelle/pebble.nix/archives/<commitHash>.tar.gz;
  # Get the hash by running "nix-prefetch-url --unpack <url>" on the tarball url
  sha256 = "<tarballHash>";
}
```

This is unnecessary if you're using flakes - flakes pin other flakes
automatically.

## Usage

### Development shells

A development shell will give you access to all the tools needed to develop,
build and test Pebble apps, including the `pebble` CLI tool, an ARM toolchain,
and the Pebble emulator. You can customize the environment by passing the
following arguments to the dev shell:

- `devServerIP`: The default development server IP. You can find this in the
  Pebble app.
- `emulatorTarget`: The default target to start the Pebble emulator for.
- `nativeBuildInputs`: Any extra tools to use during development.

## Future plans

- Get CI + Cachix going so users don't need to compile an ARM toolchain and
  QEMU; just pull straight from the binary cache instead
- Building "Rebble App Store ready" tarballs containing everything needed for
  publishing according to the [Rebble wiki](https://github.com/pebble-dev/wiki/wiki/Preparing-a-new-app-for-the-Rebble-App-Store)
