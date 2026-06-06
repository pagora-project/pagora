# Pagora Core

Document version: `README-en-v0.1.3`

Japanese guide: [README.ja.md](README.ja.md)

Pagora Core is a local-first manga library server designed for fast browsing of a local comic library.

Pagora Core runs as a service on one Linux machine. Reading is done through a Web browser. You can read from a browser on the same PC, or from another PC, tablet, or smartphone on the same LAN or VPN.

This release is the first public binary distribution for Linux x86_64. Source code is not included. Users install, update, and uninstall Pagora Core by extracting the `tar.gz` package and running the included public scripts.

## Purpose

Pagora Core prioritizes the following:

- Fast page turning
- A local library server running on a Linux machine
- Reading through a Web browser
- Access from the same PC, another device on the same LAN, or a VPN connection
- Local-file-based operation
- No cloud dependency
- Offline use
- A complete reading experience with Core alone

Account management, cloud sync, bookstores, and social features are outside the scope of Core.

## Runtime environment

The first public release of the Pagora Core server runs on the following environment:

```text
Linux x86_64
Linux environment with systemd
```

The main expected environments are Debian-based Linux distributions such as Debian, Ubuntu, Linux Mint, and MX Linux.

The Pagora reading UI is used through a Web browser. It can be accessed from a browser on the same PC, or from another device on the same LAN or VPN.

## Supported formats

Pagora Core officially supports the following formats:

```text
zip
cbz
```

PDF, rar, 7z, and direct image-folder reading are not included in this first Core public release.

## Package contents

The public tar.gz package mainly contains the following files:

```text
bin/pagora-server
web/
scripts/install_public.sh
scripts/update_public.sh
scripts/uninstall_public.sh
scripts/public_common.sh
config/pagora.service
build_info.json
README.md
README.ja.md
CHANGELOG.md
LICENSE
```

## Installation

Download the Linux x86_64 tar.gz package from the public release page and extract it.

```bash
tar -xzf pagora-core-linux-x86_64-v0.1.2-xxxxxxx.tar.gz
cd pagora-core-linux-x86_64-v0.1.2-xxxxxxx
sudo bash scripts/install_public.sh
```

The default port is `32117`.

To use a different port, specify it during the initial installation.

```bash
sudo bash scripts/install_public.sh --port 32118
```

## Installed locations

After installation, the Pagora Core runtime files are copied to their official locations on the OS.

```text
Application files:
  /opt/pagora

systemd service:
  /etc/systemd/system/pagora.service

Runtime configuration:
  /var/lib/pagora/config/public.env

Runtime data:
  /var/lib/pagora

Default library folder:
  /srv/manga
```

After installation, Pagora Core continues to work even if the original `tar.gz` file and extracted package directory are removed.

## Accessing Pagora

When installation succeeds, the Pagora service starts automatically.

To open Pagora on the installed PC:

```text
http://127.0.0.1:32117
```

To open Pagora from another PC, tablet, or smartphone on the same LAN or VPN:

```text
http://<IP address of the PC running Pagora>:32117
```

When accessing Pagora from another device, the PC running Pagora Core may need to allow incoming connections to the TCP port in use.

The default port is TCP `32117`. If you specified a different port with `--port`, that TCP port is the one that must be reachable.

Firewall, router, and VPN settings vary by environment. Pagora Core is intended for local access over the same LAN or VPN, not for direct exposure to the public Internet.

## If you cannot connect

First, check whether the following URL opens on the PC where Pagora Core is installed.

```text
http://127.0.0.1:32117
```

If this opens, Pagora Core is running. If another device cannot connect, check the following:

- Whether the other device is on the same LAN or VPN
- Whether the destination IP address is correct
- Whether the PC running Pagora Core allows incoming connections to the TCP port in use
- If you specified a different port with `--port`, whether you are using that TCP port

## Library folders

The default library folder is:

```text
/srv/manga
```

After placing `zip` or `cbz` files in `/srv/manga`, run `Rescan library` from Pagora Settings > Library to make them available in Pagora Core.

To use another folder, go to Settings > Library, select `Add folder` under `Library folders`, add the folder, and run `Save changes`. Then run `Rescan library`.

When you add new library files, run `Rescan library` from Settings > Library to reflect them in the title list.

Settings > Library also includes `Full rebuild`. For normal library additions or folder changes, use `Rescan library` first. `Full rebuild` is a heavier maintenance operation that rebuilds the entire library.

Updates, uninstall, and reinstall do not delete the following library folders:

```text
/srv/manga
Library folders added from Settings > Library
```

Pagora Core updates and uninstall do not delete your library files.

## Update

Download and extract the new tar.gz package, then run the update script from the extracted directory.

```bash
tar -xzf pagora-core-linux-x86_64-vX.Y.Z-xxxxxxx.tar.gz
cd pagora-core-linux-x86_64-vX.Y.Z-xxxxxxx
sudo bash scripts/update_public.sh
```

The update replaces the application files in `/opt/pagora`. The previous application files are backed up under `/var/lib/pagora/backups/`.

The following are not changed:

```text
/var/lib/pagora
/srv/manga
Library folders added from Settings > Library
/var/lib/pagora/config/public.env
```

Existing port settings and library settings are preserved.

## Uninstall

Uninstall Pagora Core by running the script from the installed environment.

```bash
sudo bash /opt/pagora/scripts/uninstall_public.sh
```

Uninstall removes the following:

```text
/opt/pagora
/etc/systemd/system/pagora.service
```

The following are not removed:

```text
/var/lib/pagora
/srv/manga
Library folders added from Settings > Library
```

Your library files are not deleted.

## Reinstall

If you uninstall and then install Pagora Core again, existing settings under `/var/lib/pagora` and existing library folder settings are respected.

When existing settings are present, Pagora Core does not reset the library location back to only `/srv/manga`.

## Service checks

Check service status:

```bash
systemctl --no-pager status pagora
```

Show recent logs:

```bash
journalctl -u pagora -n 100 --no-pager
```

Follow logs:

```bash
journalctl -u pagora -f
```

## Health check

The public scripts use the following health check:

```text
http://127.0.0.1:<port>/api/v2/health
```

Expected response example:

```json
{
  "app": "pagora",
  "ok": true,
  "apiVersion": "v2"
}
```

## Known limitations

- The first Core public release targets Linux x86_64 only.
- Public binaries for Windows, macOS, and Linux arm64 are not included in this release.
- PDF, rar, 7z, and direct image-folder reading are not supported.
- Direct exposure to the public Internet is not recommended.
- Pagora Core is intended for local access over LAN or VPN.

## License

See the included `LICENSE` file for license terms.

## Changelog

See the included `CHANGELOG.md` file for release history.
