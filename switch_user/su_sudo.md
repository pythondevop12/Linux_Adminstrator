# Linux Administration: User Switching and Privilege Escalation

This guide covers the essential tools for managing user identity and elevated privileges in a Linux environment.

---

## 1. The `su` Command (Switch User)

The `su` (substitute user) command allows you to run commands with the privileges of another user. By default, it switches to the **root** user.

* **`su [username]`**: Switches to the specified user but keeps the current shell's environment variables (like `PATH` and `HOME`).
* **`su - [username]`**: (Recommended) Switches to the user and starts a **login shell**. This loads the target user's profile and environment, ensuring you are working in their specific environment.
* **Authentication**: To use `su`, you must know the **password of the target user**.

---

## 2. The `sudo` Command (SuperUser DO)

`sudo` allows a permitted user to execute a command as the superuser or another user, as defined in the `sudoers` file.

* **Key Advantage**: Unlike `su`, users authenticate with **their own password**, not the root password. This provides better security and accountability through logging.
* **`sudo -i`**: Grants an interactive login shell as root (similar to `su -`).
* **`sudo -u [user] [command]`**: Runs a specific command as a different user (e.g., `sudo -u www-data ls /var/www`).

---

## 3. The `/etc/sudoers` File

This is the configuration file that controls who can run what and where. It should **never** be edited with a standard text editor like `vim` or `nano` directly.

### The `visudo` Utility
Always use the `visudo` command to edit the sudoers file. 
* **Validation**: It performs a syntax check before saving. A syntax error in `/etc/sudoers` can lock every user out of root access.
* **Locking**: It prevents multiple administrators from editing the file simultaneously.



### Syntax Breakdown
A typical entry looks like this:
`root    ALL=(ALL:ALL) ALL`

| Component | Description |
| :--- | :--- |
| **User** | The user the rule applies to (`root`, `username`, or `%group`). |
| **Host** | The hosts where the rule applies (usually `ALL`). |
| **(User:Group)** | The users/groups that can be impersonated (usually `ALL:ALL`). |
| **Commands** | The specific commands allowed (e.g., `/usr/bin/apt`, or `ALL`). |

---

## 4. Common Administrative Patterns

### Granting Full Sudo Access
Instead of adding individual lines for every user, Linux admins usually add users to a specific group that already has sudo privileges:
* **Ubuntu/Debian**: Add user to the `sudo` group.
* **CentOS/RHEL/Fedora**: Add user to the `wheel` group.

```bash
# Example: Adding a user to the sudo group
usermod -aG sudo username
