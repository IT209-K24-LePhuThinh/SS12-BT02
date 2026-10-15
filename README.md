# BT02 - Tự động đóng gói và phát hành Docker Image khi gắn Git Tag (v*.*.*)

## Workflow

File `.github/workflows/release.yml` chạy khi push tag khớp `v*.*.*`, đăng nhập GHCR, trích tag bằng `docker/metadata-action@v5` và push image bằng `docker/build-push-action@v5`.

## Tag đã push

```bash
git tag v1.0.0
git push origin v1.0.0
 * [new tag]         v1.0.0 -> v1.0.0
```

## Kết quả chạy thật

Run: https://github.com/IT209-K24-LePhuThinh/SS12-BT02/actions/runs/38771155023

```text
release-image    success    2026-10-15 16:41:51 - 16:42:08 UTC

ref: refs/tags/v1.0.0
Login Succeeded!
tags: ghcr.io/it209-k24-lephuthinh/ss12-bt02:1.0.0
org.opencontainers.image.version=1.0.0
org.opencontainers.image.revision=364ea378993cb14b304f648463de47ccc31392b3
docker buildx build --tag ghcr.io/it209-k24-lephuthinh/ss12-bt02:1.0.0 \
  --tag ghcr.io/it209-k24-lephuthinh/ss12-bt02:latest --push .
```

Image đã đẩy lên GHCR: `ghcr.io/it209-k24-lephuthinh/ss12-bt02:1.0.0`

## Cấu hình chính

```yaml
on:
  push:
    tags:
      - 'v*.*.*'
```
