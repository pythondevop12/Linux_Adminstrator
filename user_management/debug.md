## 5. Group Management

### Basic Administration
* **Create Group:** `sudo groupadd <group_name>`
* **Delete Group:** `sudo groupdel <group_name>`
* **View User's Groups:** `groups <username>`

---

### Membership Management
* **Add to a single group:** `sudo usermod -aG <group_name> <username>`
* **Add to multiple groups:** `sudo usermod -aG <group1>,<group2>,<group3> <username>`
* **Change Primary Group:** `sudo usermod -g <group_name> <username>`
* **Remove user from a group:** `sudo gpasswd -d <username> <group_name>`

---

### Group Types & Flags
| Flag | Type | Context |
| :--- | :--- | :--- |
| `-g` | **Primary** | The user's default group (usually matches username). |
| `-G` | **Secondary** | Supplementary groups for specific access (e.g., `sudo`, `developers`). |
| `-a` | **Append** | **Critical:** Use with `-G` to add groups without removing existing ones. |

---

### Implementation Notes
* **Warning:** If you run `usermod -G` without the `-a` flag, you will overwrite the user's current group list, potentially stripping them of `sudo` privileges.
* **Persistence:** Group membership updates require a **logout/login** cycle. Use `newgrp <group_name>` to refresh permissions in the current terminal session without logging out.

## 6. System Identity Files

In Linux, user and group information is stored in plain-text files located in the `/etc/` directory. Understanding these files is essential for manual auditing and troubleshooting.

### Core Identity Files
| File Path | Purpose | Access Level |
| :--- | :--- | :--- |
| `/etc/passwd` | Contains basic user account information. | Readable by all users. |
| `/etc/shadow` | Contains encrypted passwords and account aging info. | Readable only by **root**. |
| `/etc/group` | Defines groups and lists their members. | Readable by all users. |
| `/etc/gshadow` | Contains secure group information (rarely used). | Readable only by **root**. |

---

### File Structure Breakdown

#### 1. The `/etc/passwd` File
Each line represents one user, divided into 7 fields:
`username : password_placeholder : UID : GID : GECOS : home_directory : shell`

* **x**: Indicates the password is stored securely in `/etc/shadow`.
* **UID**: User ID (0 is always root).
* **GID**: Primary Group ID.
* **GECOS**: User comments (Full name, phone, etc.).
* **Shell**: The default CLI (e.g., `/bin/bash` or `/sbin/nologin` for system accounts).

#### 2. The `/etc/shadow` File
Used to protect password hashes from "John the Ripper" style offline attacks.
`username : hashed_password : last_change : min_age : max_age : warn : inactive : expire`



---

### Key Differences: passwd vs. shadow

* **Security:** `/etc/passwd` is world-readable because many tools need to map UIDs to names. `/etc/shadow` is restricted because it contains the cryptographic hashes of passwords.
* **Content:** `passwd` focus is on **identity and environment**; `shadow` focus is on **authentication and security policy**.

---

### Useful Commands for Auditing
* **Check for users without passwords:**
    `sudo awk -F: '($2 == "") {print $1}' /etc/shadow`
* **List all human users (UID 1000+):**
    `awk -F: '$3 >= 1000 && $3 != 65534 {print $1, $3}' /etc/passwd`
* **Verify file integrity:**
    `pwck` (Checks `/etc/passwd` and `/etc/shadow` for syntax errors or missing files).