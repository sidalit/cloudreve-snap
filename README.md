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

Open [http://127.0.0.1:5212](http://127.0.0.1:5212) and register an account. The service starts automatically; the first registered account becomes the administrator. Create it before exposing Cloudreve to your network. Registration stays open for further accounts, as upstream ships it; turn it off in the dashboard settings if you do not want that.

## Configure

| Setting | Accepted values | Default |
| --- | --- | --- |
| `listen` | `host:port`, `[IPv6]:port` or `:port` for all interfaces; port `1`–`65535` | `127.0.0.1:5212` |

```bash
sudo snap get cloudreve
sudo snap set cloudreve listen=:5212
```

Changes restart an enabled or running service. `sudo snap unset cloudreve listen` restores the default. After changing the address, update the site URL under Settings, Basic in the dashboard. Use an HTTPS reverse proxy for internet access.

Other settings go in upstream's [configuration file](https://docs.cloudreve.org/overview/configure): an external MySQL or PostgreSQL database, Redis, TLS or slave node mode. Run `sudo snap restart cloudreve` after editing it. The snap sets `Listen` in `[System]` and `DBFile` in `[Database]` itself, so these two keys have no effect in the file. With `[SSL]` or `[UnixSocket]` configured, Cloudreve listens on the address of that section instead of `listen`.

Data lives under `/var/snap/cloudreve/common/data`:

| Path | Contents |
| --- | --- |
| `conf.ini` | Configuration file |
| `cloudreve.db` | SQLite database |
| `uploads` | Files of the default storage policy |
| `avatar`, `temp` | Avatars and temporary files |

Upstream's maintenance commands, such as `eject`, work on the same data; `sudo cloudreve --help` lists them. The server itself only runs as the service.

Optional access to folders under `/media`, `/mnt` and `/run/media` for storage policies:

```bash
sudo snap connect cloudreve:removable-media
```

## Limitations

- Cloudreve itself makes thumbnails for JPEG, PNG and GIF images and music covers only. VIPS, FFmpeg, FFprobe, LibreOffice and LibRaw are not included, so the generators and the media information extractor that need them cannot be used.
- Remote download needs a separately installed aria2 or qBittorrent; neither is included. Run it as root on the same machine and set the node's temporary download directory to a folder inside `/var/snap/cloudreve/common`.
- Local storage policies can only use folders inside `/var/snap/cloudreve/common` or, once `removable-media` is connected, under `/media`, `/mnt` and `/run/media`. These folders must be owned by root: the service runs as root but cannot override file permissions. Home folders are not reachable.
- Databases and Redis are reached over TCP. Their Unix sockets in host locations such as `/run` are blocked; a socket placed inside `/var/snap/cloudreve/common` works.
- In Cloudreve 4.19.1, `cloudreve master-key get` and `rotate` stop with "Invalid Semantic Version", an upstream defect; `master-key generate` works.
- This is the community edition. Updates come through snapd. Back up the data folder before upgrading: `snap revert` does not undo database migrations.

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
