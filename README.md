# MySQL / Percona Datadir Safe Shifter

A production-ready Bash script to safely relocate the MySQL/Percona `datadir` to another directory (e.g., from `/var/lib/mysql` to `/home/mysql` due to lack of disk space) on **RHEL, AlmaLinux, Rocky Linux, and CentOS**.

## 🛡️ The Paradox & The Solution

**The Problem:** Moving the MySQL data directory directly to `/home/` usually breaks the system due to modern security constraints. Systemd sandboxing (`ProtectHome=true`) completely hides the `/home` directory from the MySQL service, and SELinux blocks access. Disabling `ProtectHome=false` leaves your system vulnerable.

**The Solution:** This script moves the data physically to `/home/mysql` but uses an **OS-level BIND mount** to map it back to `/var/lib/mysql`. 
* **Security is preserved:** `ProtectHome=true` remains active.
* **Compatibility is maintained:** MySQL, SELinux, Percona XtraBackup, and system updates still interact with the default `/var/lib/mysql` path.

## ✨ Features

- 🛑 Safely stops MySQL/MariaDB/Percona services before migration.
- 📦 Moves data using `cp -a` to preserve all file attributes, owners, and ACLs.
- 🔄 Automates persistent BIND-mount configuration via `/etc/fstab`.
- 🔒 Registers the new physical path in SELinux policies (`mysqld_db_t`).
- ⚡ Automatically updates systemd and restarts the service.
- 🛡️ Built-in safety checks (checks for root execution, source existence, and prevents overwriting non-empty target directories).

## 🚀 Usage

### 1. Clone the repository
```bash
git clone https://github.com
cd mysql-safe-move
```

### 2. Make the script executable
```bash
chmod +x move_mysql.sh
```

### 3. Run as root
```bash
sudo ./move_mysql.sh
```

## 📋 Requirements

- OS: RHEL 8/9, AlmaLinux, Rocky Linux, CentOS.
- Access: `root` privileges.
- Standard tools used: `semanage`, `restorecon`, `mount`.

## 📄 License

This project is licensed under the MIT License.
