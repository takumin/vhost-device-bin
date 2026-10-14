# vhost-device-bin
Prebuilt binaries of [vhost-device](https://github.com/rust-vmm/vhost-device) built with `cargo zigbuild`.

| Target | Minimum glibc |
| --- | --- |
| `x86_64-linux-gnu` | 2.25 |
| `aarch64-linux-gnu` | 2.25 |

Each release asset is a tarball laid out for `/usr/local`:

```
bin/vhost-device-can
bin/vhost-device-console
bin/vhost-device-i2c
bin/vhost-device-input
bin/vhost-device-rng
bin/vhost-device-rtc
bin/vhost-device-scmi
bin/vhost-device-scsi
bin/vhost-device-spi
bin/vhost-device-vsock
share/man/man1/*.1
share/doc/vhost-device/*
share/doc/vhost-device-*/README.md
```

Only the crates without native library dependencies are built.
`vhost-device-gpio` (libgpiod), `vhost-device-gpu` (virglrenderer, gfxstream), `vhost-device-sound` (ALSA, PipeWire, GStreamer) and `vhost-device-media` (V4L2) are not included.
Man pages are only available for the crates that provide them upstream.

# About release workflow
vhost-device tags each crate separately and infrequently, so this repository tracks the `main` branch instead.
Releases are tagged with the committer date of the vhost-device commit in SemVer form, such as `v2026.10.8` for a commit made on 2026-10-08 (UTC).
If several commits share the same date, only the first one released keeps the tag.

Renovate updates `version.json` when the `main` branch of vhost-device moves.
When the update is merged, CI on the main branch builds the binaries and creates the release, unless the tag already exists.
To build another vhost-device commit, run the CI workflow manually with `build-version`.

```console
$ gh workflow run ci -f build-version=b269fabeea543c95a415bcb57ba5f8e4bea94c32
```
