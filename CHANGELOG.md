# Changelog

All notable user-visible changes to Pagora Core are recorded in this file.

## v0.1.1 - 2026-06-06

Initial public binary release of Pagora Core for Linux x86_64.

### Added

- Linux x86_64 binary tar.gz distribution.
- Public installation, update, and uninstall scripts.
- systemd service integration.
- Local-first manga library server.
- Browser-based reading UI for same-PC, LAN, and VPN access.
- Support for local `zip` and `cbz` manga archives.
- Default TCP port `32117`.
- Default library folder at `/srv/manga`.
- Application files installed under `/opt/pagora`.
- Runtime data stored under `/var/lib/pagora`.
- Runtime configuration stored at `/var/lib/pagora/config/public.env`.
- Health check endpoint at `/api/v2/health`.
- Public package metadata for Core version, API namespace, and API contract.

### Changed

- Public distribution is provided as a prebuilt Linux x86_64 tar.gz package.
- Public install and update flows do not depend on Git, Cargo, Node.js, or npm on the user machine.
- Public update replaces the installed application body under `/opt/pagora` while preserving runtime data, port settings, and library folders.
- User-facing version display separates Core version from API namespace.

### Removed

- Source code is not included in the public binary package.
- PDF is not part of this Core public release.
- rar, 7z, and direct image-folder browsing are not part of this Core public release.

### Notes

- Direct exposure to the public Internet is not recommended.
- Pagora Core is intended for local access over LAN or VPN.
- Updates, uninstall, and reinstall do not delete `/srv/manga` or library folders added from Settings > Library.
