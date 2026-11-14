# os-images

[![GitHub License](https://img.shields.io/static/v1?label=License&message=Apache-2.0&color=blue)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://makeapullrequest.com)

> [!NOTE]
> This is a fork of [ironcore-dev/os-images](https://github.com/ironcore-dev/os-images).
> It is adapted to package **only** the `kvm-capi` flavor of Garden Linux,
> which is built from the
> [gonzolino/gardenlinux](https://github.com/gonzolino/gardenlinux) fork until
> the flavor is available upstream.

## Overview
The `os-images` repository specializes in automating the release and publication of Operating System (OS) image artifacts as OCI (Open Container Initiative) images. This project streamlines the process of pushing these OS images to the project's GitHub Container Registry (`ghcr.io`) via GitHub Actions.

### Published Images

Images in this fork are published according to the following scheme:

```
ghcr.io/gonzolino/gardenlinux/capi:$version-kvm[-$arch]
```

* `$version`: The version of the [gardenlinux](https://gardenlinux.io) base image.
* `$arch`: Either `amd64` or `arm64`. Currently only `amd64` is published, as
  the gardenlinux fork builds the `kvm-capi` flavor for `amd64` only.

The architecture can be omitted to fetch a manifest listing all permutations:

```
ghcr.io/gonzolino
└── /gardenlinux/capi
    ├── :$version-kvm
    └── :$version-kvm-amd64
```

## Workflow

The main publishing workflow (`publish-gardenlinux.yml`) runs weekly on Sundays
and can also be triggered manually via `workflow_dispatch`. It:

1. **Resolves the version** to publish: uses the manually provided version or
   discovers the latest release of
   [gonzolino/gardenlinux](https://github.com/gonzolino/gardenlinux) from the
   GitHub API. Scheduled runs skip publishing if the version has already been
   published.
2. **Downloads the `kvm-capi` artifact** from the Garden Linux GitHub release
   for all configured architectures (currently `amd64` only).
3. **Builds a multi-arch OCI image** using
   [`ironcore-image`](https://github.com/ironcore-dev/ironcore-image).
4. **Pushes the image** to `ghcr.io` with per-arch sub-manifest tags.

## Contributing
Contributions to enhance and broaden the scope of the os-images project are encouraged. Please ensure all changes are well-tested before submission.

## License

This project is licensed under the [Apache License 2.0](LICENSE).
