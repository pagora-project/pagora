# Changelog

All notable user-visible changes to Pagora Core are recorded in this file.

## v0.1.2 - 2026-06-06

Reader view mode update for Pagora Core.

### Added

- Added Reader View Mode switching from the Reader screen.
- Added `Standard`, `Two Page`, `Wide Manga`, `Landscape`, and `Auto` view modes.
- Added reading direction switching for `RTL` and `LTR` reading.
- Added local browser persistence for the last selected Reader View Mode and reading direction.
- Added automatic reset of Reader View Mode to `Standard` when the Library screen is shown, preventing one book's temporary view mode from unintentionally carrying into the next book.
- Added demo library patterns for Reader View Mode validation, including Two Page, Wide Manga, Landscape, and Auto mode samples.

### Changed
a
- Reader View Mode is now controlled from an in-reader panel instead of the Settings screen.
- `Two Page` mode now displays archive pages as view units without changing server page count or API behavior.
- `Wide Manga` mode now fits each half-page segment as its own readable area, so the active segment is centered in the browser viewport.
- `Landscape` mode now fits the full image to the available reader area while preserving aspect ratio and avoiding overflow.
- `Auto` mode now treats horizontal pages as Landscape-style full-page display and does not automatically split pages as Wide Manga.
- Auto playback now advances by Reader ViewUnit rather than assuming one archive page per step.
- Auto playback behavior was improved for Two Page, Landscape, Auto, and Wide Manga view modes.
- The mobile Reader View Mode panel layout was adjusted so Reading Direction remains visible more reliably.

### Fixed

- Fixed Landscape pages staying at their natural image size instead of fitting the browser area.
- Fixed Auto mode retaining the old Landscape sizing behavior for horizontal pages.
- Fixed Wide Manga segment positioning so the displayed half-page is centered instead of aligning the manga center line to a browser edge.
- Fixed Wide Manga Auto Playback stopping after internal segment normalization.
- Fixed Wide Manga Auto Playback across mixed normal pages and wide pages.
- Fixed several Two Page entry and image loading issues that could cause an empty reader view or a single centered page when entering from the Library.

### Notes

- Reader View Mode remains a client-side Reader UI feature.
- This release does not change the Core API, `pageCount`, archive indexing, or server-side image generation.
- Pagora still keeps the rule `1 archive entry = 1 page`; view modes only change how pages are displayed in the browser.

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
