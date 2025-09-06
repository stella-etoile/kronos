# Kronos

Kronos is a smarter file transfer wrapper that unifies `rsync`, `rclone`, and standard Unix tools under a single command. It chooses safe defaults, automatically selects the right strategy for the job, and verifies data integrity. The goal is to make moving files over SSH reliable, efficient, and simple.

## Overview

Existing tools like `scp`, `rsync`, and `rclone` are powerful, but each has trade-offs. Kronos removes the guesswork:

- **Small files and directories** → transferred directly with `rsync`.
- **Large single files** → split into parts and fetched in parallel with `rclone` over SFTP.
- **Integrity** → every transfer is verified with SHA-256.
- **Resume support** → already downloaded parts are skipped automatically.
- **Cleanup** → temporary parts are removed after a verified transfer.

Kronos is predictable and transparent: it warns about thresholds, ports, and part counts, while leaving you in control.

## Features

- **Automatic mode selection**
  - Uses `rsync` for small files and directories.
  - Switches to parallel split mode for large files.

- **Parallel transfers**
  - Select part count with `--parts N`.
  - Or chunk by size with `--size CHUNK`.
  - `--parts` without a number defaults to 8 equal parts.

- **Safe defaults**
  - Defaults to port 22 (warns if unspecified).
  - Defaults to 1 part (direct `rsync`).

- **Integrity verification**
  - SHA-256 ensures the merged file matches the source.

- **Resumable and clean**
  - Skips completed parts.
  - Removes temporary parts on success.

## Why Kronos?

- **Simpler**: one interface instead of juggling `scp`, `rsync`, and `rclone`.  
- **Safer**: checksum verification and resume support reduce risk of corruption and wasted bandwidth.  
- **Faster**: parallel mode can saturate network bandwidth better than single-stream transfers.  
- **Transparent**: prints clear hints and warnings so you know what it’s doing.

## Installation

On Arch Linux:

```bash
yay -S kronos-cli-git
```

Or clone the repo and run the script directly:

```bash
git clone https://github.com/stella-etoile/kronos
cd kronos
./kronos --help
```

## Usage

Basic transfer:

```bash
kronos user@host:/remote/path/file.txt ~/Downloads
```

Split into 6 parts:

```bash
kronos --parts 6 user@host:/remote/path/bigfile.mkv ~/Downloads
```

Transfer with chunk size:

```bash
kronos --size 500M user@host:/remote/path/bigfile.iso ~/Downloads
```

## License

MIT — see [LICENSE](LICENSE).
