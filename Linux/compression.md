# Compression Commands in Linux

File compression is an essential skill for managing storage and transferring files efficiently. This guide outlines common Linux commands for compressing and extracting files and directories.

---

## Commands Overview

### 1. `zip`: Create a Compressed Zip Archive

- **Command:**
  ```bash
  zip -r new_folder.zip folder_name
  ```
  - Recursively compresses the `folder_name` directory into `new_folder.zip`.

- **Extract:**
  ```bash
  unzip new_folder.zip
  ```
  - Extracts the contents of `new_folder.zip` into the current directory.

---

### 2. `tar`: Create and Extract Tar Archives

#### Compress:

- **Command:**
  ```bash
  tar -cvzf folder.tar.gz folder_name
  ```
  - Compresses `folder_name` into a `.tar.gz` archive using gzip.

#### Extract:

- **Command:**
  ```bash
  tar -xvzf folder.tar.gz
  ```
  - Extracts the contents of `folder.tar.gz`.

---

### Key Options for `tar`:

- **c**: Compress files into an archive.
- **v**: Display verbose information about the compression/extraction process.
- **z**: Use gzip compression.
- **f**: Specify the archive file name.
- **x**: Extract files from an archive.

---

## Summary Table

| Command               | Purpose                            |
|-----------------------|------------------------------------|
| `zip -r`             | Recursively compress files/folders |
| `unzip`              | Extract a zip archive             |
| `tar -cvzf`          | Compress using tar and gzip       |
| `tar -xvzf`          | Extract tar.gz archive            |

---

### Tips

- Use `man <command>` (e.g., `man tar`) to explore additional options.
- Verify archive contents before extraction using:
  ```bash
  tar -tvzf folder.tar.gz
  ```
- Avoid overwriting files during extraction by inspecting first or using flags like `--keep-old-files`.

Master these commands to efficiently manage file storage and transfer tasks on Linux systems!

