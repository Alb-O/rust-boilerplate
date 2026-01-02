# rust-boilerplate

Rust project boilerplate for use with [imp.gits](https://github.com/imp-nix/imp.gits).

## Usage

Add to your `.imp/gits/config.nix`:

```nix
{
  vars = {
    project_name = "my-project";
    crate_name = "my-project";
  };

  injections = [
    {
      name = "rust-boilerplate";
      remote = "https://github.com/YOUR_USER/rust-boilerplate.git";

      # Synced - generic configs that rarely need customization
      use = [
        "rust-toolchain.toml"
        "rustfmt.toml"
        "clippy.toml"
        "nix/outputs/perSystem/formatter.nix"
      ];

      # Boilerplate - spawned once, customize as needed
      boilerplate.dir = "boilerplate";
    }
  ];
}
```

Then run:

```bash
imp-gits init
```

## What's Included

### Synced Files (via `use`)

Generic configs kept in sync - changes here propagate to all projects:

| File | Purpose |
|------|---------|
| `rust-toolchain.toml` | Pinned nightly version (shared across projects) |
| `rustfmt.toml` | Formatting preferences |
| `clippy.toml` | Clippy configuration |
| `nix/outputs/perSystem/formatter.nix` | treefmt with rustfmt |

### Boilerplate Files (via `boilerplate.dir`)

Spawned once from `boilerplate/` directory, then yours to customize:

| File | Purpose |
|------|---------|
| `.cargo/config.toml` | Linker config (mold) - customize per platform |
| `Cargo.toml` | Workspace config - add your dependencies |
| `flake.nix` | Nix flake entry point |
| `nix/outputs/perSystem/devShells.nix` | Dev shell - add project deps here |
| `nix/outputs/perSystem/checks.nix` | CI checks |
| `nix/outputs/perSystem/packages.d/00-rust.nix` | Package build |
| `crates/core/` | Starter crate scaffold |

## Template Variables

Uses `@var@` syntax (nix-style substituteAll convention). Variables are substituted when boilerplate files are spawned.

| Variable | Description |
|----------|-------------|
| `project_name` | Project name (used in flake description) |
| `crate_name` | Main crate name (used in Cargo.toml and nix package) |

## Customization

After `imp-gits init`, edit the boilerplate files for your project:

```nix
# nix/outputs/perSystem/devShells.nix
packages = [
  # ... existing packages ...
  pkgs.pkg-config
  pkgs.openssl
  pkgs.sqlite
];
```
