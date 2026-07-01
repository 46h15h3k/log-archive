# Log Archive Tool

A simple CLI tool that compresses logs from any directory into a timestamped `.tar.gz` archive and keeps an activity log of every run.

---

## Features

- Accepts any log directory as a CLI argument
- Produces a timestamped archive: `logs_archive_YYYYMMDD_HHMMSS.tar.gz`
- Stores all archives under `~/logs_archive/`
- Appends a one-line entry to `~/logs_archive/archive_activity.log` after each run
- Skips unreadable files gracefully instead of aborting

---

## Requirements

- Bash 4+ (macOS ships Bash 3 — use `brew install bash` or `zsh` is fine too)
- Standard Unix utilities: `tar`, `date`, `du` (present on all Linux/macOS systems)

---

## Installation

```bash
# 1. Clone the repository
https://github.com/46h15h3k/log-archive.git

# 2. Make it executable
chmod +x log-archive

# 3. Move it somewhere on your PATH (optional, for system-wide use)
sudo mv log-archive /usr/local/bin/log-archive
```

---

## Usage

```bash
log-archive <log-directory>
```

### Examples

Archive the system log directory:
```bash
log-archive /var/log
```

Archive a custom application log directory:
```bash
log-archive /opt/myapp/logs
```

Archive the current directory:
```bash
log-archive .
```

---

## Output

| Path | Description |
|------|-------------|
| `~/logs_archive/logs_archive_<timestamp>.tar.gz` | Compressed archive |
| `~/logs_archive/archive_activity.log` | Append-only run history |

### Sample archive_activity.log

```
2024-08-16 10:06:48 | source=/var/log | archive=logs_archive_20240816_100648.tar.gz | size=4.2M
2024-08-17 02:00:01 | source=/var/log | archive=logs_archive_20240817_020001.tar.gz | size=3.8M
```

---

## Scheduling with cron

To run the archiver automatically every day at 2 AM:

```bash
# Open your crontab
crontab -e

# Add this line (adjust the path to wherever log-archive lives)
0 2 * * * /usr/local/bin/log-archive /var/log
```

---

## Notes

- The tool requires **read permission** on the target directory. Run with `sudo` if archiving system logs owned by root.
- Archives are stored in `~/logs_archive/` by default. The directory is created automatically on first run.
- Files that cannot be read (permission denied) are silently skipped so the rest of the archive is still created.
