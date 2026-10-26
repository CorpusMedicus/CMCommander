# CM Commander

A free dual-pane file manager for Linux by CorpusMedicus.

Keep source and destination in view, move files between folders, and work with archives in one familiar desktop app.

## Downloads

AppImages and their checksums are provided on the [Releases page](https://github.com/CorpusMedicus/CMCommander/releases). The first CM Commander testing preview is being prepared.

This repository hosts downloads, documentation and issue reports. Application source code is not included. Third-party source attachments contain bundled dependency sources only.

## Features

- Two independent file panes with folder trees, navigation history, search and sorting.
- Copy and move files with overwrite prompts and drag-and-drop.
- JPEG/PNG thumbnails, ZIP/7z archives and bundled 7-Zip.
- Browse network shares already connected through the desktop.
- Temporary RAM workspace and encrypted vault workflow.
- Optional KDE Connect sharing.

## Requirements and installation

Built for Linux Mint 22.x on Intel/AMD 64-bit (x86_64), with glibc 2.39 or later. Other distributions and older Mint versions have not been validated.

Download the AppImage, allow execution in Properties → Permissions, then double-click it. If FUSE is unavailable:

```sh
chmod +x CMCommander-1.0-x86_64.AppImage
./CMCommander-1.0-x86_64.AppImage --appimage-extract-and-run
```

Save SHA256SUMS beside your downloaded files and verify:

```sh
sha256sum --ignore-missing -c SHA256SUMS
```

## Help and feedback

Read [Help](HELP.md) for usage and known limitations. Report reproducible problems through [Issues](https://github.com/CorpusMedicus/CMCommander/issues), including your Linux Mint version and steps to reproduce. Keep backups and test unfamiliar operations with disposable files.

## License

Free to use, copy and redistribute for personal or commercial purposes; supplied as is without warranty. See [LICENSE.txt](LICENSE.txt). Third-party components retain their own licenses; see [source collection notes](THIRD-PARTY-SOURCES.md).
