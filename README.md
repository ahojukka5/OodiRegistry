# OodiRegistry

Public Julia package registry for the Oodi scientific-computing ecosystem.

**Status: bootstrapped, empty.** This repository contains Julia registry
metadata, not the source code of a Julia package. No packages or versions
have been registered yet, and `Pkg.add("Oodi")` will not work through this
registry until appropriate versions have been reviewed and registered.

A public registry does **not** make any source repository public. However,
the registry metadata itself is public: registered package names, UUIDs,
repository URLs, versions, dependencies, and compatibility information may
be visible to everyone. Do not add private, employer-owned, customer-owned,
or unpublished package metadata without explicit authorization.

## Add the registry to Julia

```julia
using Pkg

Pkg.Registry.add(url = "https://github.com/ahojukka5/OodiRegistry.git")
Pkg.Registry.status()
```

Julia Pkg can use this registry alongside the official `General` registry.
If `General` is not yet installed on this machine, add it explicitly:

```julia
Pkg.Registry.add("General")
```

The registry only catalogs versions and their source locations. Access to
a private source repository still requires separate Git credentials and
GitHub authorization. Do not put credentials or tokens in registry files.

## Development is not registration

Active cross-package work may use `Pkg.develop` and exact Git revisions.
A registered version is a reviewed, immutable release candidate that users
and reproducible applications can depend on. Each package keeps its own
independent version history; this registry does not impose synchronized
version numbers or put Oodi itself in charge of every ecosystem package.

For a scientific study, preserve the exact `Project.toml`,
`Manifest.toml`, code revision, input identities, and other experiment
provenance. A registry is not a substitute for an immutable experiment
environment.

## Maintainer workflow (provisional)

The ecosystem-wide SemVer, compatibility, release and approval contract
is being defined separately. **Do not register the first package merely
because this empty registry is now installable.**

When the package owner has explicitly approved a version and its
visibility, the intended review-based workflow is:

1. Ensure the package has a clean Git working tree and a committed
   `Project.toml` with an appropriate `name`, `uuid`, `version`,
   `[compat]` and dependency declarations.
2. Run package tests and applicable downstream/Oodi integration tests
   against the exact source revisions. Record the tested combination.
3. With the registry installed as a Git clone, create a feature branch
   in its local checkout; install `LocalRegistry.jl` in a **separate
   maintainer environment**, not in the package's runtime dependencies.
4. Activate the package's project and have LocalRegistry create the
   registry commit without automatically pushing to the integration
   branch:

   ```julia
   using LocalRegistry
   register(; registry = "OodiRegistry", push = false)
   ```

5. Review the resulting metadata (including repository URL,
   `git-tree-sha1`, dependency and compatibility entries), push the
   registry feature branch, and open a pull request for human approval.
   A release/tag and registry merge require the agreed release process.
6. After approval, verify installation from this registry in a clean
   Julia environment and record the resolved versions. Check both
   public and, where applicable, separately authenticated private
   installation paths.

`LocalRegistry.register` does **not** check that all dependencies are
available in a registry, or that a combination of versions works
scientifically. Those are separate validation responsibilities. Registry
updates must not silently replace the source tree of an existing
version.

Packages located in subdirectories of a repository can be registered,
but require explicit handling and testing of their package paths.

## References

- [Julia Pkg: registries](https://pkgdocs.julialang.org/v1/registries/)
- [Julia Pkg: compatibility](https://pkgdocs.julialang.org/v1/compatibility/)
- [LocalRegistry.jl](https://github.com/GunnarFarneback/LocalRegistry.jl)
- [LocalRegistry registration details](https://github.com/GunnarFarneback/LocalRegistry.jl/blob/master/docs/register.md)
