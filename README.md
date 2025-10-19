# 🛡️ Tarka Backup Script

A **portable, incremental backup tool** using `tar`, `rsync`, `gpg`, and `sshfs`. Designed for reliability, minimalism, and transparent encryption—**without exposing plaintext on remote systems**.

---

## 🚀 Features

* **Incremental & Full backups** (`tar -g`)
* **Encrypted backups** using `GPG` (AES256)
* **Remote support via SSHFS**
* **Automatic pruning** of old backups (`keep`)
* **Restoration & verification** of archives
* **File integrity checking** with SHA256
* **Human-readable backup identifiers** using `ispell` word list
* **Portable**: works with POSIX shells (uses GNU `tar`, `rsync`, etc.)

---

## 📦 Requirements

* `rsync`, `gpg`, `tar` (GNU), `sshfs`
* `dateutils` (for precise date math)
* `moreutils` (for `sponge`)
* `ispell` (for generating readable backup tags)
* SSH access for remote usage

---

## 🔧 Commands

```bash
./tarka.sh CMD REPO [SRC] [HASH] [NR]
```

### 🔁 Backup

* `full` — Perform a full backup
* `increment` — Create an incremental backup

### 🔍 Other Operations

* `list` — List backups in the repository
* `check` — Verify checksums of backup files
* `restore SRC HASH [NR]` — Restore a specific backup
* `delete SRC HASH [latest]` — Delete a specific or latest backup
* `keep SRC Y M W` — Keep only X yearly/monthly/weekly backups (auto prune)

---

## 🧠 Backup File Naming

Backups are named as:

```
<escaped_path>~<timestamp>=<hash>+<nr>[--<tag>].tar.gpg
```

This structure helps:

* Uniquely identify backup groups
* Manage incremental chains
* Maintain readability

---

## 🔐 Encryption

Set password via:

```bash
export TARKA_PASSWORD="your-password"
```

Or enter interactively when prompted.

---

## 🧰 Environment Variables

* `TARKA_NO_WARN=1` — Disable confirmation prompts (use with caution)
* `TARKA_PASSWORD=...` — Set encryption password
* `TARKA_RESTORE_DEST=...` — Set custom restore path

---

## 🌐 Remote Repository Usage

Mount remote backup repo using SSH:

```bash
./tarka.sh full user@host:/path/to/remote backups/mydir
```

Backups are encrypted **locally** before being synced to the remote.

---

## 📁 Example Usages

### Full backup to local repo:

```bash
./tarka.sh full /mnt/backup mydata/
```

### Incremental backup to remote:

```bash
./tarka.sh increment user@host:/backup mydata/
```

### List all backups:

```bash
./tarka.sh list /mnt/backup
```

### Restore:

```bash
./tarka.sh restore /mydata <hash>
```

### Keep latest 2 yearly, 3 monthly, and 4 weekly backups:

```bash
./tarka.sh keep /mydata 2 3 4
```

---

## 🧪 TODO

* [ ] Show sizes in `list`
* [ ] Handle directories with spaces
* [ ] Improve `keep` robustness
* [ ] Add progress indicators

---

## 📝 Philosophy

> "Maximize the lines of code **not written**."

This script minimizes assumptions, maximizes transparency, and works with composable UNIX tools.

---

Let me know if you want a Markdown version with formatting or a man-page-style version.
