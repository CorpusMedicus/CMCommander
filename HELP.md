# CM Commander Help

## About and License
CM Commander 1.0
Author: CorpusMedicus
Copyright (c) 2026 CorpusMedicus

FREE USE AND DISTRIBUTION
CM Commander is free to use, copy and redistribute, without restriction, for personal or commercial purposes. No payment or donation is required.

AS IS — NO WARRANTY
CM Commander is provided "as is", without warranty of any kind, express or implied, including warranties of merchantability, fitness for a particular purpose and noninfringement. To the extent permitted by applicable law, the author is not liable for claims, damages or other liability arising from the software or its use, including loss of data.

TESTING STATUS
CM Commander 1.0 still needs broader testing across different Linux systems, drives and network environments. Bugs may remain. Keep backups of important files and test unfamiliar operations with disposable copies before relying on them.

THIRD-PARTY COMPONENTS
These permissions apply to CMCommander. Third-party components, including 7-Zip when supplied with the application, remain subject to their own licenses and notices.

## Getting started
CM Commander is a two-pane file manager for Linux. Use either pane to browse folders, then copy or move selected files to the other pane.

Each pane has a folder tree and a file list. Click a folder in the tree or double-click a folder in the list. The Desktop, Documents, Downloads, Pictures, Music and Videos buttons jump to those locations and highlight the selected folder.

Each pane has a Back button. The file and file-list background menus also include Back and Up. Back returns to previously visited folders in that pane; the two panes have independent histories of up to 100 locations for the current session. Unavailable locations are skipped. Up opens the parent folder and is disabled at the filesystem root or Network overview.

Path boxes keep up to five recent paths. Type or choose a path, then press Enter to navigate. A leading ~ means your home folder.

Right-click a folder, a file or the file-list background for actions. The available actions depend on where you clicked. Show Hidden Files reveals names beginning with a dot. Search filters names in the displayed folder; it is not a recursive search. Click a column heading to sort.

## Image thumbnails
Click the eye button above either file list to toggle image previews in that pane independently. The open-eye button shows thumbnails; the closed-eye button hides them and restores compact rows. Filenames, sorting, selection and drag-and-drop remain available.

JPEG (.jpg and .jpeg) and PNG are supported. TIFF and SVS are not previewed in this first version. Files over 32 MB, JPEG images over 64 megapixels, PNG images over 16 megapixels, unsupported images, and files that fail to decode keep their normal icons. Larger JPEGs use reduced-size decoding before the final thumbnail resize.

Previews are requested for painted rows and generated one at a time in a separate decoder process. Each preview fits within 80 by 80 pixels. The in-memory cache holds at most 192 entries, with a queue of at most 48 requests. The decoder has a 512 MB address-space limit, a 5-second CPU limit and a 10-second elapsed timeout. SMB preview loading may therefore fall back to an icon on a slow connection.

No thumbnail files are written to disk. Switching folders, refreshing, disabling previews, and beginning vault ejection or RAM cleanup clear cached previews and cancel pending work. As with the RAM workspace, operating-system swap may still contain memory contents. Thumbnail mode starts off by default and is not saved between sessions.

## Copying and moving
Select one or more items in the file list. Use Copy or Cut, navigate to the destination pane or folder, then use Paste. Cut moves items; Copy keeps the originals.

If the destination exists, Yes allows replacement, No skips that item, and Cancel stops the remaining batch. Cancel does not undo items already transferred. Folder conflicts merge their contents: matching files can be overwritten, while destination-only files are kept.

Files are copied into temporary files beside the destination, then renamed into place after the copy and flush succeed. A failed copy leaves an existing destination file intact. Preserving Unix file permissions and file access/modification timestamps (to whole seconds) is best effort: network and device filesystems may use their own permissions instead. Data-write, flush, close and final-rename failures still stop the transfer. This needs enough destination space for the new copy alongside any old file.

A failed or skipped cut item stays available to retry. A partly moved folder may have completed files at the destination and unfinished files at the source. Review the reported paths before retrying. Folder moves are not all-or-nothing transactions.

The folder-tree clipboard and the file-list clipboard are separate. For a predictable multi-file workflow, use Copy/Cut/Paste within the file lists.

Recursive transfers currently reject symbolic links and special files when encountered. A same-filesystem folder rename can move a folder without traversing its contents.

## Desktop links and device sharing
Send to Desktop in the file-list menu creates symbolic links to selected files or folders. Originals stay in place. Existing Desktop names are preserved by adding a numbered suffix. Moving or deleting the original breaks its link; deleting the link does not delete the original. Links to RAM workspaces or network mounts only work while their targets are available.

Share Via KDE Connect opens the KDE Connect device chooser for selected files. Install KDE Connect on this computer and the receiving device, pair them, and make sure they can reach each other. The menu item is hidden when kdeconnect-handler is unavailable. Multiple files can be sent together; zip folders before sharing. KDE Connect handles device discovery and transfer progress.

Copy/Cut/Paste currently uses CM Commander's internal clipboards. Copying files directly between CM Commander and Mint Files or the Desktop through the system clipboard is not implemented yet.

## Keyboard shortcuts
These shortcuts apply while a file list has keyboard focus:

Enter — open the selected item
Backspace — go to the parent folder
F2 — rename
Ctrl+C — copy
Ctrl+X — cut
Ctrl+V — paste
Delete — move to Trash
Shift+Delete — permanently delete

In a path box, Enter navigates to the entered path. After using a shortcut button, click the file list before using file-list shortcuts.

## Deleting files
Move to Trash requests the desktop trash service. Trash availability depends on the destination filesystem; network shares may not support it. Use Mint Files to inspect or restore trashed files.

Delete permanently removes selected items after confirmation. CM Commander does not provide an undo operation for permanent deletion.

Secure Delete invokes an overwrite-based tool. It cannot guarantee erasure on SSDs, copy-on-write filesystems, snapshots, backups or remote storage. Do not treat it as a guarantee that every copy is gone.

## Network shares
Connect to your SMB share using Mint Files first. Then expand Network in either CM Commander folder tree and select the connected share, shown as share name on server.

CM Commander reuses the desktop connection and its authentication. This version does not create connections, discover servers, manage network passwords or disconnect shares. Use Mint Files for those tasks.

If you connect a share after opening CM Commander, right-click Network and choose Refresh. Only desktop shares exposed through GVfs appear here; other mounts may be accessible through File System or Devices / Removable.

Visited directories are cached in memory and shared between panes. Cached listings expire after 15 seconds when requested again. Refresh requests current data immediately. File changes invalidate affected listings. There is no live monitoring of changes made on another computer.

The first visit can still pause while the server responds. If a server disconnects, reconnect through Mint Files and refresh the folder. Check any transfer error before retrying.

## ZIP and 7z archives
Use Zip on selected files to create a ZIP archive, or Zip on a folder-tree item to create a 7z archive. Unzip extracts a selected archive into a newly created folder. Extensionless archives use a name ending in _extracted; existing destination names receive numbered suffixes. Existing folders are not merged or overwritten. Duplicate names inside an archive are renamed automatically. On failure, a single error reports the destination where partial files may remain.

Password prompts are masked. For ordinary archives, leaving the password blank requests no encryption on creation. Cancelling the password prompt cancels that operation.

Encrypted ZIP creation uses AES-256. The tested 7-Zip version requires ASCII ZIP passwords; use a 7z archive or a vault for Unicode passwords. Encrypted 7z folders use filename encryption.

Passwords are sent through a private pipe to 7-Zip, not in process arguments. A failed archive operation reports an error. Keep your source files and verify an archive before relying on it as your only copy.

## RAM workspace
RAM drive ON atomically creates /dev/shm/SecureRAMDisk_<numeric-user-id> with owner-only permissions (0700). Existing paths must be real directories owned by the current user with those permissions; symlinks, wrong ownership and unsafe permissions are refused. Old username-based workspaces are not automatically adopted or removed. Save and eject any vault in an older running version before switching builds. The pane RAM buttons open it, and an active Secure RAM Disk entry appears in both folder trees. This uses the system's shared-memory filesystem; it does not reserve a fixed amount of memory.

RAM Drive OFF removes the workspace and its contents after confirmation. RAM Drive OFF is disabled while a vault is mounted. Use Eject Vault to save the archive and remove its RAM workspace together.

RAM storage is temporary and can be lost on reboot. Depending on system configuration, its contents may be swapped to disk. Keep enough free RAM/storage for your work and keep important originals elsewhere.

When closing with an ordinary RAM workspace, you can remove it, leave it in RAM, or cancel. Leaving it in RAM is not a persistent backup.

## Vaults and passwords
A CM Commander .vault file is a standard encrypted 7z archive. CM Commander adds a temporary RAM workspace and a managed save/eject workflow; it does not introduce a stronger encryption algorithm than 7-Zip.

1. Create Vault: choose a name, a save location and a long, unique passphrase. Confirm the passphrase.
2. Vault Mount: choose the saved vault and enter its password. Before extraction, CM Commander reads the uncompressed file sizes using your password and adds a 20% buffer. That estimate must fit both available system RAM and free /dev/shm space. If the check fails or sizes cannot be determined, mounting stops before changing the workspace. Only one vault can be mounted at a time.
3. Work with files inside that workspace. Files beside the .vault archive are not part of the vault and are not encrypted by it.
4. Eject Vault: re-enter the password. CM Commander verifies it against the existing archive, builds and tests a replacement, replaces the saved archive, then removes the RAM workspace.

The size check does not reserve memory. Other applications and later edits can consume space after the check; extraction failures are still handled.

Save and close files in other applications before ejecting; CM Commander cannot control an external editor's unsaved changes.

Closing with a mounted vault offers seal and exit, discard changes with a second confirmation, or cancel. Discard keeps the previously saved archive and removes the working changes.

Your vault password is not retained between operations or recoverable by CMCommander. Keep it somewhere safe. Passwords may contain Unicode for 7z/vaults, but cannot contain line breaks or NUL and are limited to 1024 UTF-8 bytes.

Passwords travel through a private pipe and owned password buffers are wiped on a best-effort basis. This cannot guarantee erasure of every GUI or child-process memory copy, or protection from privileged malware.

Files are decrypted while the vault is open. RAM-backed storage may be swapped to disk. The saved archive protects its contents when sealed; the working session is not encrypted RAM.

## Vault recovery
If compression, password verification or archive verification fails, CM Commander keeps the session open and preserves the original archive and working files. Correct the problem and try again. Do not discard the session if you need unsaved work.

Recovery archives receive unique .tmp names beside the saved vault. A failed attempt may leave a recovery file; incomplete or verification-failed files must not be assumed usable. Keep the reported path when investigating a failure.

If the new vault is saved but RAM cleanup fails, CM Commander reports that state. Eject retries cleanup only; it will not reseal a partly removed workspace over the saved archive. Do not edit remaining workspace files in this state.

If the mounted workspace or original archive is missing, sealing cannot proceed normally. Preserve whatever remains and inspect the saved/recovery archives before deleting anything.

These safeguards handle failures during a running session. Automatic recovery of a mounted session after a crash is not implemented. A crash can leave a RAM workspace behind; a reboot can lose it. Backups are still necessary.

## Requirements and current limits
Archive and vault features require the 7z executable. The CM Commander AppImage already bundles 7-Zip: no separate installation is needed for that build. When running a standalone executable or building from source, install a package providing the 7z command through Mint Software Manager or the system package tools. If missing, CM Commander reports that archive features are unavailable.

KDE Connect is optional and is not bundled in the AppImage. Install KDE Connect through Mint Software Manager (package kdeconnect) to enable Share Via KDE Connect. CM Commander checks for its kdeconnect-handler executable each time the file menu opens. A KDE Plasma desktop is not required.

Network browsing depends on a desktop GVfs connection. Opening files and terminals depends on installed desktop applications and utilities.

Some menu entries are unfinished: file Properties has no action, and folder Sharing currently routes to folder properties. New Word and New Spreadsheet create empty placeholders with Office extensions, not valid Office documents. The right terminal shortcut currently delegates to the left pane's terminal action.

Background network directory loading and network connection management are not implemented. For important operations, keep backups and review error messages.
