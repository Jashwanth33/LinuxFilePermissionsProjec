# Linux File Permissions Project

[![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)](https://linux.org)
[![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnubash&logoColor=white)](https://gnu.org/software/bash/)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

> Comprehensive guide to Linux file permissions, ownership management, and Access Control Lists (ACLs).

## Permission System Overview

`mermaid
graph TB
    subgraph "Linux Permission System"
        User[User/Owner]
        Group[Group]
        Others[Others]
    end

    subgraph "Permission Types"
        Read[Read r=4]
        Write[Write w=2]
        Execute[Execute x=1]
    end

    subgraph "Commands"
        chmod[chmod]
        chown[chown]
        chgrp[chgrp]
        setfacl[setfacl]
        getfacl[getfacl]
    end

    User --> Read
    User --> Write
    User --> Execute
    Group --> Read
    Group --> Write
    Group --> Execute
    Others --> Read
    Others --> Write
    Others --> Execute

    chmod --> User
    chmod --> Group
    chmod --> Others
    chown --> User
    chgrp --> Group
`

## Permission Matrix

`mermaid
graph LR
    subgraph "Permission Bits"
        R[Read - 4]
        W[Write - 2]
        X[Execute - 1]
    end

    subgraph "Combinations"
        RWX[7 - rwx]
        RW[6 - rw-]
        RX[5 - r-x]
        R[4 - r--]
        WX[3 - -wx]
        W[2 - -w-]
        X[1 - --x]
        NONE[0 - ---]
    end

    R --> RWX
    W --> RW
    X --> RX
    RW --> RWX
    RX --> RWX
`

## Permission Change Flow

`mermaid
flowchart TD
    A[Start] --> B{Command Type}
    
    B -->|Symbolic| C[Symbolic Mode]
    B -->|Octal| D[Octal Mode]
    
    C --> C1[u/g/o/a]
    C1 --> C2[+/-/=]
    C2 --> C3[r/w/x]
    
    D --> D1[User Digit]
    D1 --> D2[Group Digit]
    D2 --> D3[Other Digit]
    
    C3 --> E[Apply Permissions]
    D3 --> E
    
    E --> F{Recursive?}
    F -->|Yes| G[Apply to Directory]
    F -->|No| H[Apply to Target]
    
    G --> I[Update inode]
    H --> I
    
    I --> J[Done]
`

## Special Permissions

`mermaid
graph TB
    subgraph "Special Permissions"
        SUID[SUID - 4000]
        SGID[SGID - 2000]
        StickyBit[Sticky Bit - 1000]
    end

    subgraph "Effects"
        SUID_E[Run as file owner]
        SGID_E[Run as file group]
        Sticky_E[Only owner can delete]
    end

    subgraph "Examples"
        SUID_EX["/usr/bin/passwd"]
        SGID_EX["/usr/bin/newgrp"]
        Sticky_EX["/tmp"]
    end

    SUID --> SUID_E
    SGID --> SGID_E
    StickyBit --> Sticky_E
    
    SUID_E --> SUID_EX
    SGID_E --> SGID_EX
    Sticky_E --> Sticky_EX
`

## ACL Flow

`mermaid
flowchart TD
    A[File Access Request] --> B[Check Owner]
    B -->|Match| C[Apply Owner Permissions]
    B -->|No Match| D[Check ACL User Entry]
    D -->|Match| E[Apply ACL Permissions]
    D -->|No Match| F[Check Group]
    F -->|Match| G[Apply Group Permissions]
    F -->|No Match| H[Check ACL Group Entry]
    H -->|Match| I[Apply ACL Group Permissions]
    H -->|No Match| J[Apply Other Permissions]
    J --> K{Access Granted?}
    E --> K
    G --> K
    I --> K
    K -->|Yes| L[Access Allowed]
    K -->|No| M[Access Denied]
`

## Project Structure

`
LinuxFilePermissionsProjec/
│
├── scripts/
│   ├── permission_manager.sh           # Main permission management script
│   ├── permission_checker.sh           # Check current permissions
│   ├── acl_manager.sh                  # ACL management
│   ├── user_manager.sh                 # User/group management
│   ├── backup_permissions.sh           # Backup permissions
│   └── restore_permissions.sh          # Restore permissions
│
├── examples/
│   ├── basic/
│   │   ├── example1_basic.sh           # Basic chmod usage
│   │   ├── example2_chown.sh           # Ownership changes
│   │   └── example3_recursive.sh       # Recursive changes
│   │
│   ├── intermediate/
│   │   ├── example4_octal.sh           # Octal notation
│   │   ├── example5_special.sh         # Special permissions
│   │   └── example6_umask.sh           # Umask settings
│   │
│   └── advanced/
│       ├── example7_acl.sh             # ACL examples
│       ├── example8_default_acl.sh     # Default ACLs
│       └── example9_complex.sh         # Complex scenarios
│
├── exercises/
│   ├── beginner/
│   │   ├── exercise1.sh                # Fix permissions
│   │   ├── exercise2.sh                # Set up web server
│   │   └── exercise3.sh                # Secure scripts
│   │
│   ├── intermediate/
│   │   ├── exercise4.sh                # Multi-user setup
│   │   ├── exercise5.sh                # Shared directories
│   │   └── exercise6.sh                # Automated backups
│   │
│   └── advanced/
│       ├── exercise7.sh                # Production server
│       ├── exercise8.sh                # Security audit
│       └── exercise9.sh                # Compliance setup
│
├── configs/
│   ├── default_permissions.conf        # Default configurations
│   ├── secure_permissions.conf         # Secure configurations
│   └── web_server_permissions.conf     # Web server configs
│
├── docs/
│   ├── PERMISSIONS_GUIDE.md            # Comprehensive guide
│   ├── ACL_GUIDE.md                    # ACL documentation
│   ├── CHEATSHEET.md                   # Quick reference
│   └── BEST_PRACTICES.md              # Security best practices
│
├── tools/
│   ├── audit_permissions.py            # Permission auditing tool
│   ├── fix_permissions.py              # Auto-fix permissions
│   └── visualize_permissions.py        # Visualize permissions
│
├── tests/
│   ├── test_permissions.sh
│   └── test_acl.sh
│
├── README.md
└── LICENSE
`

## Quick Reference

### Basic Commands

| Command | Description | Example |
|---------|-------------|---------|
| chmod | Change permissions | chmod 755 file.sh |
| chown | Change owner | chown user:group file |
| chgrp | Change group | chgrp group file |
| ls -la | List permissions | ls -la /etc |
| umask | Set default | umask 022 |

### Octal Values

| Octal | Permission | Description |
|-------|------------|-------------|
| 0 | --- | No permissions |
| 1 | --x | Execute only |
| 2 | -w- | Write only |
| 3 | -wx | Write + execute |
| 4 | -- | Read only |
| 5 | -x | Read + execute |
| 6 | w- | Read + write |
| 7 | wx | Full permissions |

### Symbolic Mode

| Symbol | Meaning |
|--------|---------|
| u | User/owner |
| g | Group |
| o | Others |
|  | All (ugo) |
| + | Add permission |
| - | Remove permission |
| = | Set exact permission |

## Examples

`ash
# Give owner full permissions, group read/execute, others read
chmod 754 file.txt

# Symbolic mode - add execute for all
chmod a+x script.sh

# Change ownership
chown john:developers project/

# Set ACL for specific user
setfacl -m u:bob:rwx shared_dir/

# Set default ACL for new files
setfacl -d -m g:team:rx shared_dir/

# Find files with 777 permissions
find / -perm 777 -type f 2>/dev/null

# Backup ACLs
getfacl -R /data > acl_backup.txt

# Restore ACLs
setfacl --restore=acl_backup.txt
`

## Best Practices

1. **Principle of Least Privilege** - Give minimum required permissions
2. **Avoid 777** - Never use chmod 777 on production
3. **Use Groups** - Organize users into groups
4. **Regular Audits** - Check permissions regularly
5. **Use ACLs** - For complex permission requirements
6. **Document Changes** - Keep records of permission changes

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

MIT License

## Author

**Jashwanth** - [GitHub](https://github.com/Jashwanth33)