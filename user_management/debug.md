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