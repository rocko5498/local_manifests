# OnePlus 11R (udon) — tested LineageOS 23.2 source set

This branch pins the exact clean source revisions used for the successful
`lineage-23.2-20260728-UNOFFICIAL-udon.zip` build.

ROM SHA-256:

```
fb62e792d5c75af1c59cfd93592709521e9e5a302b3ea130d89e43cb1c2849ec
```

## Status

- Compiles successfully and boots.
- Fingerprint is broken; the fingerprint option was absent from Settings in
  tester feedback. No decisive runtime logs were captured.
- Alert slider is broken. The source includes the upstream initial-state
  population change, but runtime testing showed that it did not fix the
  feature.
- NFC packaging was added to this build, but NFC operation was not confirmed
  on-device.
- This is an experimental, unsupported device bring-up, not a stable release.

No speculative post-build fingerprint or alert-slider changes are included in
this branch.

## Source composition

`default.xml` pins every non-LineageOS repository by commit SHA. Important
choices include:

- udon device and vendor trees derived from Udon-F-up
- Arman-derived SM8450 common device tree, with its prebuilt-kernel changes
  already reverted in the pinned revision
- pjgowtham SM8450 source kernel, modules, and device trees
- Arman Oplus hardware tree
- pinned Soong, Qualcomm common, AGM, and Dolby dependencies

Every project declared by this manifest is hosted publicly under
`github.com/rocko5498`. The modules, device-tree, Oplus hardware, and Dolby
repositories are GitHub forks retaining the exact upstream commits used by the
tested build. The Dolby checkout is retained for exact source-environment
parity, although the final build log does not show modules being consumed from
that repository.

## Build

Start from a clean LineageOS 23.2 checkout, copy `default.xml` into
`.repo/local_manifests/`, then run:

```bash
repo sync --force-sync -c -j$(nproc --all)
source build/envsetup.sh
breakfast udon
m -k bacon
```

After every sync, verify the checked-out revisions against `default.xml` before
building. A force sync discards uncommitted local changes.
