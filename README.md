# VCBrowser

VCBrowser is the Windows browser runtime used by [ViralCloner](https://github.com/viralcloner/Windows-ViralCloner) for browser profiles and automation. It is distributed separately as a portable ZIP and downloaded by the desktop application when needed.

[Download releases](https://github.com/viralcloner/VCBrowser/releases) · [ViralCloner for Windows](https://github.com/viralcloner/Windows-ViralCloner) · [Report an issue](https://github.com/viralcloner/VCBrowser/issues)

## Install through ViralCloner

1. Open ViralCloner and go to **Settings**.
2. Find the **VCBrowser** section and select the download button.
3. Wait for the download, integrity verification, and extraction to finish.

ViralCloner fetches the latest stable release from this GitHub repository and selects the Windows ZIP matching the application's architecture. No ViralCloner account or GitHub token is required for public release downloads, and the download process does not use `viralcloner.com`.

The default installation directory is:

```text
%APPDATA%\viralcloner\vcbrowser
```

An existing installation is reused. The download action installs a missing browser; it does not automatically replace an installed version. ViralCloner application updates are distributed separately through [Windows-ViralCloner releases](https://github.com/viralcloner/Windows-ViralCloner/releases).

## Manual installation

Download the appropriate Windows ZIP from a release's **Assets** section and extract its complete contents. For use with ViralCloner, the resulting executable should be located at:

```text
%APPDATA%\viralcloner\vcbrowser\VCBrowser.exe
```

Keep the executable together with all bundled runtime files. GitHub's automatically generated **Source code (zip)** and **Source code (tar.gz)** downloads are not compiled browser packages.

Close ViralCloner and VCBrowser and back up existing browser data before manually replacing an installation.

## Release package

Publish a **portable `.zip` file** containing the complete browser runtime. Use a filename that identifies the target architecture:

| Architecture | Asset filename |
| --- | --- |
| Windows x64 | `VCBrowser-win-x64.zip` |
| Windows ARM64, if built | `VCBrowser-win-arm64.zip` |
| Windows x86, if built | `VCBrowser-win-ia32.zip` |

Only publish architectures that have been built and tested. These names describe the download convention, not a guarantee that every architecture is available.

Put `VCBrowser.exe` at the archive root, or place the complete runtime inside a single enclosing folder. A typical layout is:

```text
VCBrowser-win-x64.zip
├── VCBrowser.exe
├── version.txt
├── <browser-version>/
├── <other required runtime files and directories>
└── <upstream licenses and third-party notices>
```

The exact runtime files depend on the build. Include every dependency required to run the browser, along with all applicable license notices. Do not package only the executable or include symbolic links.

Set `version.txt` to the actual Chromium/browser version, for example:

```text
149.0.7827.54
```

Exclude personal profiles, cookies, browsing history, sessions, caches, generated profile icons, credentials, and private configuration.

## Publish a release

1. Prepare and test a clean portable Windows build.
2. Add `version.txt` and the required upstream license notices.
3. Create the ZIP using the appropriate asset filename above.
4. Create a public, stable GitHub release in this repository, using a version tag such as `v149.0.7827.54`.
5. Upload the compiled ZIP as a release asset and make the release the latest stable release.
6. Verify that the GitHub Releases API provides the asset's byte `size` and `digest` in the form `sha256:<64 hexadecimal characters>`.
7. Test installation through ViralCloner with a fresh application data directory.

The desktop downloader uses:

```text
https://api.github.com/repos/viralcloner/VCBrowser/releases/latest
```

It ignores drafts and prereleases and verifies both the downloaded byte count and SHA-256 checksum before extracting the archive. Assets without a SHA-256 digest are rejected; re-upload an asset if its digest is missing.

## Troubleshooting

| Problem | What to check |
| --- | --- |
| No stable release is available | Publish a public release with a compiled Windows ZIP attached. |
| No matching Windows ZIP | Check the asset extension and architecture suffix. |
| Missing digest or checksum mismatch | Re-upload the correct ZIP and verify its GitHub asset metadata. |
| Archive does not contain `VCBrowser.exe` | Package the complete runtime with the executable at the root or inside one enclosing folder. |
| Existing browser is not replaced | Automatic download only installs a missing browser. |

When reporting a problem, include the release tag, Windows version, architecture, and error message. Remove credentials and personal data from logs before sharing them.

## Source code and licensing

Browser components and bundled dependencies retain their respective licenses. Consult the license files and third-party notices shipped with each release. The ViralCloner desktop application's ISC license does not automatically apply to VCBrowser binaries or their dependencies.

A compiled ZIP is not source code. Browser releases must be accompanied by the corresponding source, modifications, and build instructions where required by their licenses. Claims about open-source availability should refer to the actual published browser source and its license.
