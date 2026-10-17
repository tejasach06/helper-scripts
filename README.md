# Helper Scripts

Production-engineered utilities for Linux root-cause analysis, high-throughput log extraction, and Proxmox VE cluster inventory management.

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
