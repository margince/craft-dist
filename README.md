# craft-dist

Release channel for **craft**, the Margince code-craftsmanship gate.

This repository holds **no source** — only released binaries and their
checksums, published automatically when a version is tagged in the gate's own
repository, which is private.

## Why the binary is public when the source is not

The gate has to run where no credential exists: on a contributor's laptop, in a
pre-push hook, and in CI on a pull request from a fork. An artifact reachable
only with a token would quietly stop covering exactly those cases, and a gate
that stops covering something without going red is worse than no gate. So the
artifact is public and the implementation is not.

## Using a release

Each release carries four binaries and a `checksums.txt`:

    craft_<version>_darwin_arm64
    craft_<version>_darwin_amd64
    craft_<version>_linux_arm64
    craft_<version>_linux_amd64

Verify before you run it — checking a digest after execution proves nothing:

```sh
curl -fsSL -o craft \
  https://github.com/margince/craft-dist/releases/download/<version>/craft_<version>_darwin_arm64
shasum -a 256 craft          # compare against checksums.txt
chmod +x craft && ./craft version
```

Consuming repositories pin all four digests in their own tree rather than
trusting a download, so the builds are reproducible from the tag:
`CGO_ENABLED=0`, `-trimpath`, `-ldflags="-s -w"`.

`craft rubric` prints the standard the gate applies; `craft` with no arguments
lists the subcommands.

## Licensing

`LICENSE` ships as an asset on every release, so the terms travel with the binary
rather than living somewhere you have to go and find.

It is a narrow grant, `LicenseRef-Margince-Craft-1.0`: you may obtain the binary
and run it to review, analyse or check source code — on your own machine, in CI
you control, and against code you do not own, which is what gating a pull request
actually requires. It does not grant redistribution, modification or reverse
engineering, and it does not publish the source.
