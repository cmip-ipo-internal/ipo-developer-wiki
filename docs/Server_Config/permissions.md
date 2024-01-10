# Managing Directory Permissions

## Introduction

In Linux, ensuring proper access control is vital for maintaining a secure and organized system.  
## Steps to Assign Permissions, Users and Groups

### 1. Identify the Directories

Begin by identifying the directories for which you want to modify permissions. 

### 2. Select permissions you wish to assign
Here is a table of numerical permissions in Linux:

| Permission | Numeric Value | Representation |
|------------|---------------|-----------------|
| ---        | 0             | No permission   |
| --x        | 1             | Execute         |
| -w-        | 2             | Write           |
| -wx        | 3             | Write + Execute |
| r--        | 4             | Read            |
| r-x        | 5             | Read + Execute  |
| rw-        | 6             | Read + Write    |
| rwx        | 7             | Read + Write + Execute |

These numerical values are used with the `chmod` command to set permissions for the owner, group, and others. (defined below)

#### How to read: 

The permissions are combined to define the application we are interested in. 

- Owner: Read (4) + Write (2) + Execute (1) = 7
- Group: Read (4) + Write (2) = 6
- Others: Read (4) = 4


### 2. Use `chmod` to Modify Permissions

The `chmod` command is used to change file and directory permissions. The basic syntax is:

```bash
chmod [options] permissions file/directory
```

- **Options**: Common options include `-R` (recursively apply permissions) and numerical permissions (e.g., `chmod 755`).
- **Permissions**: Use a three-digit octal number to represent the permissions as defined previously. 

`chmod 754 file` grants read, write, and execute permissions to the owner, read and write permissions to the group, and read-only permission to others.


### 3. Use `chown` to Change Ownership

If needed, you can change the ownership of a directory using the `chown` command. The basic syntax is:

```bash
chown [options] user:group file/directory
```

- **Options**: Common options include `-R` (recursively change ownership).
- **User and Group**: Specify the new owner and group for the directory.

Example:

```bash
chown -R john:users directory_name
```

This command changes the ownership of the directory and its contents to the user 'john' and the group 'users.'


### 4. Creating a group
If a group dows not exist we can add it with 
```bash 
sudo addgroup group_name
```


You can then use: 
```bash 
getent group group_name
```
to check that the group exists. 

### 4. Use `usermod` to Add Users to Groups

The `usermod` command is used to modify user account properties, including adding users to groups. The basic syntax is:

```bash
sudo usermod -aG group_name username
```

- **-aG**: Appends the user to the supplementary group(s) specified.
- **group_name**: The name of the group to which the user will be added.
- **username**: The name of the user to be added to the group.

Example:

```bash
sudo usermod -aG developers alice
```

This command adds the user 'alice' to the 'developers' group.


