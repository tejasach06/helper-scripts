# Helper Scripts

Utilities for Linux diagnostics, log extraction, Proxmox VE inventory, and Windows profile migration.

Hosted GitHub Pages: **[https://tejasach06.github.io/helper-scripts/](https://tejasach06.github.io/helper-scripts/)**

---

## Included Tools

### 1. [fs-corruption-rca-collector](https://github.com/tejasach06/fs-corruption-rca-collector)
Non-invasive, read-only diagnostic telemetry collector for Linux filesystem corruption, I/O errors, and read-only remount incidents. Gathers live kernel ring buffers, mount state, dirty buffers, and storage controller indicators without modifying host state.

- **Stack**: Python 3.8+ (zero third-party dependencies)
- **Features**: Strictly read-only, `--selftest` mode, parallel diagnostic execution, compressed tarball bundle export.

### 2. [pmta-log-extract](https://github.com/tejasach06/pmta-log-extract)
High-performance search and extraction utility for Port25 PowerMTA accounting CSV log archives. Streams through plain CSV, Gzip, Bzip2, and Zip archives with complex boolean criteria without extracting uncompressed files to disk.

- **Stack**: Rust CLI (native speed) + fallback Python implementation
- **Features**: Dual-stack engine, streaming SIMD decompression, multi-field matching (`--orig`, `--rcpt`, `--type`, `--any`), complete test parity.

### 3. [proxmox-inventory-extract](https://github.com/tejasach06/proxmox-inventory-extract)
High-performance inventory exporter for Proxmox VE hypervisors and clusters. Extracts running and stopped QEMU VMs and LXC containers, correlates network addresses, storage backends (ZFS, LVM, Directory), OS metadata, and outputs normalized tabular CSVs.

- **Stack**: Rust CLI + fallback Python implementation
- **Features**: Asynchronous parallel fetching via Tokio, API token and ticket auth, automatic guest IP/MAC resolution, multi-pool storage volume discovery.

### 4. [windows-profile-migration](https://github.com/tejasach06/windows-profile-migration)
Back up a Windows user profile to a 7-Zip archive and merge its files into another computer's profile. Includes known folders, user-created profile directories, AppData\Roaming, standard-location Outlook PST files, and browser bookmark recovery.

- **Stack**: Windows PowerShell and 7-Zip
- **Behavior**: Skip existing destination files, resolve signed-in user folder redirection, and attempt Chromium bookmark merging.
- **Precautions**: Archives are unencrypted. Keep a separate backup and review the project's documented restore limitations before use.

## Maintain the Pages runner

The `Deploy Pages` workflow publishes the repository root without a site generator. Keep `index.html` at the root. If a custom domain is added, keep its `CNAME` file there too.

Packaging and deployment run on `vm101-helper-scripts`, the repository's rootless Podman runner on VM101. Its labels are `self-hosted`, `Linux`, `X64`, `vm101`, and `podman`. The runner does not need a Docker socket. The workflow uses SHA-pinned official actions with Node.js 24 runtimes, including the upload action called by `upload-pages-artifact`.

1. In **Settings > Actions > Runners**, confirm that the repository runner is online and has all five labels.
2. Keep its container image and Actions Runner current. Node.js 24 actions require Actions Runner 2.327.1 or later.
3. After runner maintenance, select **Actions > Deploy Pages > Run workflow** and use `main`.
4. Confirm that both jobs use the VM101 runner and that the `github-pages` deployment succeeds.

On VM101, run these commands as `tejas` to inspect or restart the runner:

```sh
	systemctl --user status github-runner-helper-scripts.service
	journalctl --user -u github-runner-helper-scripts.service -n 100
	systemctl --user restart github-runner-helper-scripts.service
```

The Quadlet is `~/.config/containers/systemd/github-runner-helper-scripts.container`. It uses the shared `localhost/github-runner:latest` image. Registration state and work files are separate directories under `/mnt/data/podman-rootless/github-runner/`: `state-helper-scripts` and `work-helper-scripts`. Preserve the registration state during image updates.

Only pushes to `main` and manual runs on `main` can publish. Do not add pull request triggers or remove the main-branch guards from this homelab runner workflow. Keep **Settings > Pages > Build and deployment > Source** set to **GitHub Actions** and keep `main` allowed in the `github-pages` environment.

To return to branch publishing, set the Pages source to **Deploy from a branch**, choose `main` and `/ (root)`, and disable `Deploy Pages`. Before this migration, no custom domain was configured and HTTPS was enforced.
