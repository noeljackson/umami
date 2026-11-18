# Codewire's build of Umami

This fork builds the image Codewire runs for its own website analytics. The
`codewire` branch is an upstream release tag plus the small changes below.
Nothing here is sent upstream.

## Changes from upstream

- **`Dockerfile`**, each marked `codewire:`:
  - the Node base is pinned by digest;
  - the runner stage runs `apk upgrade`, so the base's fixed packages land;
  - the runner stage removes pnpm after installing its script dependencies.
    Nothing at runtime calls it, and it vendors its own `undici`.
- **`.github/workflows/codewire-image.yml`:** builds linux/amd64 and
  linux/arm64 with `SKIP_BUILD_GEO=1`, pushes `ghcr.io/noeljackson/umami`, adds
  SBOM and provenance attestations, and signs keyless with cosign. It runs on
  pushes to `codewire`, on demand, and weekly.
- **This file.**

## Updating to a new upstream release

1. Fetch the release tag from upstream (fetch only).
2. Create a branch from the tag.
3. Cherry-pick this fork's commits onto it.
4. Open a pull request into `codewire` in this fork.

The image is used only after Codewire's mirror helper scans its digest and
copies it into Codewire's registry.
