# Third-party source collection

This records the evidence collected for the current local AppImage. It is not
an assertion that the distribution audit is complete.

## Collected

- **Ubuntu shared libraries:** 39 distinct source package/version pairs for the
  44 binary packages in `dependencies.tsv`. Original archives, Ubuntu patches,
  and Debian source descriptors are retained. Archive sizes and SHA-256 hashes
  were checked against each descriptor. `ubuntu/provenance.json` records exact
  Launchpad publications, download URLs, and file hashes. Descriptor signatures
  have not been independently verified; downloads used HTTPS.
- **Pascal components:** snapshots of the installed FPC 3.2.2, Lazarus 4.8.0
  (including LCL, AnchorDocking, and VirtualTreeView), BGRABitmap, and BGRAControls
  source trees. Generated object files and executable binaries are excluded.
  These are local build-tree snapshots, not a claim of bit-for-bit reproduction
  from upstream release archives. Their license files remain inside the trees.
- **7-Zip 26.03:** upstream source archive and accompanying license/readme/history
  from the existing packaging inputs. The source is also embedded in the AppImage.
- **AppImage runtime:** source commit
  `75849dce7cc37e4319b633df1f116ca895c71a12`; the runtime binary matches the digest
  in the saved upstream release metadata. libfuse 3.15.0 and squashfuse 0.5.2
  source archives match the hashes in that commit's build script. The runtime
  source includes its libfuse patch and build scripts.

## Remaining public-release review

The runtime's Docker build uses a moving Alpine 3.21 base and unpinned package
installs. The saved release metadata does not record exact musl, zlib, zstd,
allocator (mimalloc), or other statically linked package versions. Recover
that build's dependency inventory and check that all applicable notices and
source requirements are covered. The upstream runtime LICENSE lists several
libraries but does not list mimalloc; the supplied binary contains mimalloc
strings. Do not interpret this collection as completion of that review.

Check the local Pascal source snapshots against the installed units if claiming
reproducible builds. `LICENSE.txt` preserves the author's existing license wording.

## Recollect and package

Run these after building the AppImage, on the same build machine:

```sh
python3 packaging/collect_library_sources.py
python3 packaging/snapshot_pascal_sources.py
python3 packaging/collect_runtime_sources.py
python3 packaging/prepare_release.py
```

The network collectors download into `packaging/third-party-sources/` and do not
install system packages. Large source archives are release attachments, excluded
from Git. Refresh collection whenever bundled dependency versions change.

Ubuntu source archives can be unpacked with `dpkg-source -x PACKAGE.dsc` in the
matching directory. The Debian packaging contains downstream patches and build
instructions. Pascal snapshots contain their Makefiles and package project files;
the runtime archive contains `BUILD.md` and scripts. Application build steps are
in the root README.
