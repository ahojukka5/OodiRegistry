# OodiRegistry agent guidance

OodiRegistry is a **public Julia Pkg registry**, not a Julia package and
not an executable research or integration-test repository.

1. Read `README.md` and `Registry.toml` before making changes. Never
   replace the registry UUID or reuse it for another registry.
2. Do not add or update a package release without explicit human owner
   authorization and a reviewed source revision. Public registry entries
   reveal metadata about private package repositories.
3. Do not infer permission to disclose any unpublished, VTT-owned,
   third-party or confidential project. No secrets, credentials or
   private research material belong in this repository.
4. Prefer generating package metadata with established
   `LocalRegistry.jl` tooling. Do not hand-edit `Versions.toml`,
   `Deps.toml`, or `Compat.toml` to invent releases or replace the
   tree hash of an existing released version.
5. Release versioning and ecosystem compatibility rules will be
   standardized separately. The bootstrap README's proposed workflow
   is not approval to register any package.
6. Keep changes small and reviewable: issue, focused branch, tests,
   PR, human approval before merge. No automatic release on a push to
   a development branch.
7. Report tests actually run. Prefer a clean Julia environment for
   registry installation and resolver checks; explicitly note when a
   Julia executable or credentials are unavailable.
8. Do not modify CI workflows or broaden credential permissions
   without an explicit user request.
