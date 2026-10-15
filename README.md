# BT02 - Tự động đóng gói và phát hành Docker Image khi gắn Git Tag (v*.*.*)

## 1. Tạo file `.github/workflows/release.yml`

```yaml
name: Release Docker Image

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  release-image:
    name: Build and push image to GHCR
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - uses: actions/checkout@v5
      - uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - id: meta
        uses: docker/metadata-action@v5
        with:
          images: ghcr.io/${{ github.repository }}
          tags: |
            type=semver,pattern={{version}}
      - uses: docker/build-push-action@v5
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

## 2. Gắn tag và push lên GitHub

```bash
git tag v1.0.0
git push origin v1.0.0
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:IT209-K24-LePhuThinh/SS12-BT02.git
 * [new tag]         v1.0.0 -> v1.0.0
```

## 3. Kết quả trên tab Actions

```text
Release Docker Image
  Build and push image to GHCR    ✓  (2m 08s)

Build and push image to GHCR
  Run actions/checkout@v5                     ✓
  Run docker/login-action@v3                  ✓
    Logging into ghcr.io...
    Login Succeeded
  Run docker/metadata-action@v5               ✓
    tags: ghcr.io/it209-k24-lephuthinh/ss12-bt02:1.0.0
  Run docker/build-push-action@v5             ✓
    #5 exporting to image
    #5 pushing layers
    #5 pushing manifest for ghcr.io/it209-k24-lephuthinh/ss12-bt02:1.0.0
    #5 DONE 12.4s
```

## 4. Gói Image trong mục Packages

```text
Packages
  ss12-bt02
    ghcr.io/it209-k24-lephuthinh/ss12-bt02:1.0.0
    Published 1 minute ago
```
