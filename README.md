<div align="center">

<img src="snap/gui/cloudreve.png" alt="Cloudreve logo" width="128" />

# Cloudreve

**Self-hosted file management with multi-cloud support.**

[![cloudreve](https://snapcraft.io/cloudreve/badge.svg)](https://snapcraft.io/cloudreve)
[![Base: core26](https://img.shields.io/badge/Base-core26-2C8C3C?logo=canonical&logoColor=white)](https://snapcraft.io/core26)
![Confinement: strict](https://img.shields.io/badge/Confinement-strict-2C8C3C)
[![Maintainer: sidaliteir](https://img.shields.io/badge/Maintainer-sidaliteir-2C8C3C?logo=snapcraft&logoColor=white)](https://snapcraft.io/publisher/sidaliteir)
![Architectures: amd64 and arm64](https://img.shields.io/badge/Architectures-amd64%20%7C%20arm64-2C8C3C?logo=linux&logoColor=white)

[Getting Started](#getting-started) · [Configure](#configure) · [Limitations](#limitations) · [Build](#build) · [Support](#support) · [License](#license)

Unofficial snap packaging for [Cloudreve](https://github.com/cloudreve/cloudreve), a self-hosted file management and sharing system that supports multiple storage providers.

</div>

---

## Getting Started

<a href="https://snapcraft.io/cloudreve"><img alt="Get it from the Snap Store" src="https://snapcraft.io/en/dark/install.svg" width="180"></a>

```bash
sudo snap install cloudreve
```

The service starts automatically. Open [http://127.0.0.1:5212](http://127.0.0.1:5212) and register: the first account becomes the administrator, so create it before exposing Cloudreve to your network. Registration stays open afterwards, as upstream ships it; you can turn it off in the dashboard.

## Configure

| Setting | Accepted values | Default |
| --- | --- | --- |
| `listen` | `host:port`, `[IPv6]:port` or `:port` for all interfaces | `127.0.0.1:5212` |

```bash
sudo snap set cloudreve listen=:5212
```

Changes restart the service, and `sudo snap unset cloudreve listen` restores the default. After changing the address, update the site URL in the dashboard settings, and use an HTTPS reverse proxy for internet access.

To use folders under `/media`, `/mnt` or `/run/media` in storage policies:

```bash
sudo snap connect cloudreve:removable-media
```

## Limitations

- Thumbnails are made for JPEG, PNG and GIF images and music covers only: VIPS, FFmpeg, LibreOffice and LibRaw are not included.
- Remote download needs aria2 or qBittorrent installed separately, with its download folder inside `/var/snap/cloudreve/common`.
- Local storage is limited to `/var/snap/cloudreve/common` and, with `removable-media`, folders owned by root under `/media`, `/mnt` and `/run/media`. Home folders are not reachable.
- Databases and Redis are reached over TCP, or through a socket inside `/var/snap/cloudreve/common`.
- `snap revert` does not undo database migrations. Back up the data folder before upgrading.

## Build

```bash
git clone https://github.com/sidalit/cloudreve-snap.git
cd cloudreve-snap
sudo snap install snapcraft --classic
snapcraft pack
```

## Support

- **Installation or snap issues:** [open a packaging issue](https://github.com/sidalit/cloudreve-snap/issues).
- **Application bugs:** [report them upstream](https://github.com/cloudreve/cloudreve/issues).

## License

Snap packaging: [MIT](https://github.com/sidalit/cloudreve-snap/blob/main/LICENSE).
