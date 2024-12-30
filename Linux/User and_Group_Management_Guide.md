# User and Group Management Guide

This guide provides step-by-step instructions for managing users and groups in a Linux system using terminal commands. These commands require superuser privileges, so ensure you have `sudo` access before proceeding.

---

## 1. User Management

### Adding a New User
```bash
sudo useradd -m new_user
```
- **Description**: Adds a new user to the system.
- **Options**:
  - `-m`: Creates the user's home directory.
- **Example**: `sudo useradd -m alice`

### Viewing User Information
```bash
sudo cat /etc/passwd
```
- **Description**: Displays a list of all system users and their details.
- **Details Included**:
  - Username
  - User ID (UID)
  - Group ID (GID)
  - Home directory path
  - Default shell

### Setting a Password for the User
```bash
sudo passwd new_user
```
- **Description**: Sets or changes the password for the specified user.
- **Example**: `sudo passwd alice`

### Deleting a User
```bash
sudo userdel new_user
```
- **Description**: Removes the specified user from the system.
- **Additional Options**:
  - `--remove`: Deletes the user's home directory and mail spool.
- **Example**: `sudo userdel --remove alice`

---

## 2. Group Management

### Adding a New Group
```bash
sudo groupadd devops
```
- **Description**: Creates a new group.
- **Example**: `sudo groupadd engineering`

### Viewing Group Information
```bash
sudo cat /etc/group
```
- **Description**: Displays a list of all system groups and their details.
- **Details Included**:
  - Group name
  - Group ID (GID)
  - Group members

### Adding Users to a Group
#### Adding a Single User
```bash
sudo gpasswd -a new_user devops
```
- **Description**: Adds a single user to the specified group.
- **Example**: `sudo gpasswd -a alice devops`

#### Adding Multiple Users
```bash
sudo gpasswd -M user1,user2,user3 devops
```
- **Description**: Adds multiple users to the specified group.
- **Example**: `sudo gpasswd -M alice,bob,charlie devops`

### Deleting a Group
```bash
sudo groupdel devops
```
- **Description**: Deletes the specified group from the system.
- **Example**: `sudo groupdel engineering`

---

## Notes
1. Always ensure you have a backup of important system configurations before making changes.
2. Use `man <command>` to view detailed information about any command (e.g., `man useradd`).
3. When removing users or groups, double-check dependencies to avoid unintended consequences.

---


**Happy Linux Administration!**
