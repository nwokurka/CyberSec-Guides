---
up: "[[Linux MOC 🗺️]]"
---

## 📌 Project Description

Managing file permissions is a crucial aspect of Linux security. This guide explains how to check, interpret, and modify file permissions using `ls -l`, `ls -la`, and `chmod`. 

### Scenario
The research team at my organization needs to adjust file and directory permissions within the `projects` directory to align with the correct authorization levels. The current permissions do not provide the appropriate level of access, posing a potential security risk. To ensure system security, I reviewed and updated the necessary permissions by performing the following tasks:

## 🔍 View Permissions

To see the permissions of every visible file in the current directory, use:

``` Bash
ls -l
```

This command displays a detailed list of files, including their permissions, owner, group, and size.

![[Pasted image 20250311161539.png]]

To view hidden files as well, use:

```bash
ls -la
```

Hidden files (starting with `.`) will also be displayed.

![[Pasted image 20250311161927.png]]

### Types of Permissions

Linux has three types of permissions:
- **Read (r)** → Allows viewing file contents or listing directory contents.
- **Write (w)** → Allows modifying a file or adding/deleting files in a directory.
- **Execute (x)** → Allows running a file (if executable) or accessing a directory.

Permissions apply to three types of users:
- **User (Owner)** → The file creator.
- **Group** → A set of users who share permissions.
- **Other** → Everyone else on the system.

For example, `drwxrwxrwx` means a directory (`d`) where user, group, and others all have full permissions.

#### Breakdown of the 10-character string:

![[LinuxPermissions.drawio.svg|600]]
### Interpreting File Permissions

![[Pasted image 20250311161927.png]]

In the /home/researcher2/projects directory, there are five files with the following names and permissions: 

- `project_k.txt`
	- User = read, write, 
    - Group = read, write
    - Other = read, write

- `project_m.txt`
	- User = read, write
    - Group = read
    - Other = none

- `project_r.txt`
	- User= read, write
    - Group = read, write
    - Other = read

- `project_t.txt`
    - User = read, write
    - Group = read, write
    - Other = read

- `.project_x.txt`
    - User = read, write
    - Group = write
    - Other = none

There is also one subdirectory inside the projects directory named `drafts`. The permissions on `drafts` are:

- User = read, write, execute
- Group = execute
- Other = none

## ✏️ Changing File Permissions

Use the `chmod` command to update file permissions.

### Removing Group Read/Write

To change the permissions of the `project_m.txt` file so that the **group** no longer has read or write access, use the following command:

``` Bash
chmod g-r project_m.txt
```

After running `ls -l`, the updated permissions appear as: **-rw-------**

![[Pasted image 20250311163231.png]]

### Updating Permissions for a Hidden File

To modify `.project_x.txt` so that both the user and group can read but not write:

```
chmod u-w,g-w,g+r .project_x.txt
```

![[Pasted image 20250311163814.png]]


### Removing Execute Permissions from a Directory

To remove execute (`x`) permission from the `drafts` directory for the group:

``` Shell
chmod g-x drafts
```

![[Pasted image 20250311173635.png]]

⚠️ **Note:** Removing execute (`x`) permission from a directory may prevent users from accessing its contents.

---

# 📝 Summary

- Use `ls -l` and `ls -la` to check file permissions, including hidden files.
- Understand the 10-character permission string: `-rwxr-xr--`.
- Modify permissions using `chmod` with symbolic (`u,g,o,+,-`) 
- Removing execute (`x`) from directories can restrict access.

By mastering file permissions, you enhance security and control access in a Linux environment. 🚀