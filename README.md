# OodiRegistry

Public Julia package registry for the Oodi scientific-computing ecosystem.

**Registered packages:** Episteme, DelonePortable, Monge, Ritz, Irons and Oodi
at `0.1.0`; Delone at `0.1.0` and `0.1.1`. This repository contains Julia
registry metadata, not the source code of a Julia package.

A public registry does **not** make any source repository public. However,
the registry metadata itself is public: registered package names, UUIDs,
repository URLs, versions, dependencies, and compatibility information may
be visible to everyone. Do not add private, employer-owned, customer-owned,
or unpublished package metadata without explicit authorization.

## Add the registry to Julia

```julia
using Pkg

Pkg.Registry.add("General")
Pkg.Registry.add(url = "https://github.com/ahojukka5/OodiRegistry.git")
Pkg.Registry.status()
```

Julia Pkg uses both the official `General` registry and OodiRegistry. Initialize
General first, including in a completely empty depot.

The registry only catalogs versions and their source locations. Access to
a private source repository still requires separate Git credentials and
GitHub authorization. Do not put credentials or tokens in registry files.

## Install Oodi

After adding both registries, use Julia 1.13.1 or newer and authenticated Git
access to the private package sources:

```julia
using Pkg
Pkg.add(name = "Oodi", version = "0.1.0")
using Oodi
```

Oodi resolves its core owners by registered version: Delone, DelonePortable,
Episteme, Ritz and Irons. It requires Delone 0.1.1 or a compatible patch and
needs no sibling checkout or Git revision override.
Optional specialists are installed separately and retain their own source
access and qualification requirements. Installation and package loading do not
by themselves establish numerical qualification of an optional integration.

## Install Episteme

After General and this registry are installed, request Episteme by
name and version:

```julia
using Pkg
Pkg.add(name = "Episteme", version = "0.1.0")
using Episteme
```

Episteme 0.1.0 keeps the former OodiCore UUID
`7c15cd61-9c6a-4671-bc94-9960963998ac`. The Git tag `v0.1.0`
originally pointed at OodiCore commit
`3d8a44177b9d2009f1f44168608881ae604b4da1`. Replacing that tag is a
separate, already authorized release step. Installation follows the
registry tree hash rather than the tag.

## Install geometry packages

With General and OodiRegistry installed, add the packages by name:

```julia
using Pkg
Pkg.add(name = "DelonePortable", version = "0.1.0")
Pkg.add(name = "Delone", version = "0.1.1")
Pkg.add(name = "Monge", version = "0.1.0")

using Monge
@assert Monge.Geometry.volume(Monge.Modeling.box(1, 2, 3)) ≈ 6
```

DelonePortable is an independent package in the Delone repository's `portable/`
subdirectory. Pkg retrieves that subdirectory through its registry entry; no
sibling checkout is needed. Its runtime dependency is the SHA standard library,
with an optional Episteme extension.

Delone and Monge require Julia 1.10 or newer. Published native wrappers cover
Julia 1.10–1.13 on Linux x86_64 with glibc and macOS aarch64. Other native
platforms and Julia ABIs are not qualified by these releases.

Delone's portable contracts and Julia algorithms load independently of the live
Netgen provider. To use that optional provider, add its dependencies explicitly:

```julia
Pkg.add(["CxxWrap", "OCCT_jll", "Zlib_jll"])
using Delone
Delone.load_netgen!()
```

Normal native installation downloads the published artifacts. Private package
source access still requires separate Git authentication, as described above.

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
is being defined separately. **Do not register another package merely
because this registry is now installable.**

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
