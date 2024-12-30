# File Permissions in Linux

Managing file permissions in Linux is essential for ensuring security and proper access control for files and directories. This document explains the common commands and their usage related to file permissions in Linux.

---

## Understanding Linux File Permissions

File permissions in Linux are represented by a combination of three types of access:

1. **Read (r)** - Allows viewing the content of a file or listing the contents of a directory.
2. **Write (w)** - Allows modifying the content of a file or adding/deleting files in a directory.
3. **Execute (x)** - Allows executing a file (if it’s a script or binary) or accessing a directory.

Permissions are assigned to three categories:
- **Owner**: The user who owns the file.
- **Group**: Users who belong to the group assigned to the file.
- **Others**: All other users on the system.

### Numeric Representation of Permissions
Permissions are often represented numerically using a 3-digit octal value, where each digit represents the access level for **Owner**, **Group**, and **Others**.

| Number | Permission   | Binary Representation |
|--------|--------------|------------------------|
| 0      | No access    | 000                    |
| 1      | Execute      | 001                    |
| 2      | Write        | 010                    |
| 3      | Write & Execute | 011                  |
| 4      | Read         | 100                    |
| 5      | Read & Execute | 101                  |
| 6      | Read & Write | 110                    |
| 7      | Read, Write & Execute | 111          |

---

## Common Commands for Managing Permissions

### 1. `chmod`
Used to change the permissions of a file or directory.

#### Syntax:
```bash
chmod [permissions] [file_name or folder_name]
```

#### Example:
```bash
chmod 777 file.txt
```
- Grants **read, write, and execute** permissions to **Owner**, **Group**, and **Others**.

### 2. `umask`
Defines the default permission set for new files and directories.

#### Syntax:
```bash
umask [value]
```

#### Example:
```bash
umask 022
```
- Ensures new files have 755 permissions (Owner: RWX, Group: RX, Others: RX).

### 3. `chown`
Changes the ownership of a file or directory.

#### Syntax:
```bash
chown [new_owner] [file_name or folder_name]
```

#### Example:
```bash
chown john file.txt
```
- Transfers ownership of `file.txt` to the user `john`.

### 4. `chgrp`
Changes the group ownership of a file or directory.

#### Syntax:
```bash
chgrp [new_group] [file_name or folder_name]
```

#### Example:
```bash
chgrp developers file.txt
```
- Changes the group ownership of `file.txt` to the `developers` group.

---

## Visualizing File Permissions
Below is a diagram showing how permissions and ownership are structured:

```plaintext
[ - | rwx | rw- | r-- ]
|     |      |     |
|     |      |     +---> Permissions for Others
|     |      +----------> Permissions for Group
|     +------------------> Permissions for Owner
+-------------------------> File Type (- for file, d for directory)
```

For example, the permission string `-rwxr-xr--` means:
- It’s a file (`-` at the start).
- Owner has read, write, and execute permissions (`rwx`).
- Group has read and execute permissions (`r-x`).
- Others have read-only permissions (`r--`).

---

## Summary Table

| Command      | Purpose                              |
|--------------|--------------------------------------|
| `chmod`      | Modify permissions for files/directories |
| `umask`      | Set default permissions for new files |
| `chown`      | Change ownership of files/directories |
| `chgrp`      | Change group ownership of files      |

---

Properly managing file permissions enhances security and ensures files are accessible only to authorized users. Use these commands carefully to maintain a secure environment.

