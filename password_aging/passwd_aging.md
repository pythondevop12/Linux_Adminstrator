# Comprehensive Guide to Password Aging in Linux

Password aging is a security policy that enforces a "use-by date" on user credentials. In Linux, this is primarily managed through the `/etc/shadow` file and controlled via specific system utilities.

---

## 1. Why Do We Need Password Aging?

While it can be a nuisance for users, password aging serves several critical security functions:

* **Credential Rotation:** If a password is compromised via a data breach or keylogger, aging ensures the attacker's window of access is limited.
* **Compliance Standards:** Frameworks like **PCI-DSS**, **HIPAA**, and **SOC2** often mandate password rotation policies.
* **System Hygiene:** It helps identify "stale" or abandoned accounts. If a user hasn't logged in long enough for their password to expire and lock, the account is likely a candidate for deletion.
* **Mitigating Brute Force:** Periodic changes render long-term offline brute-force attacks less effective.

---

## 2. The Anatomy of `/etc/shadow`

All password aging data is stored in the `/etc/shadow` file. Each line represents a user and follows this colon-separated format:

`username:password:last_change:min_age:max_age:warn:inactive:expire:reserved`



| Field | Description |
| :--- | :--- |
| **Last Change** | Days since Jan 1, 1970 (Unix Epoch) that the password was last changed. |
| **Min Age** | Minimum days required between password changes. |
| **Max Age** | Maximum days a password is valid. |
| **Warn** | Number of days before expiration to start warning the user. |
| **Inactive** | Number of days after expiration before the account is locked. |
| **Expire** | An absolute date (in days since Epoch) when the account itself expires. |

---

## 3. Methods to Configure Password Aging

### A. The `chage` Command (Individual Users)
The `chage` (change age) tool is the most robust way to manage these settings.

* **View current status:**
    ```bash
    sudo chage -l username
    ```
* **Set Max (90 days) and Min (7 days) age:**
    ```bash
    sudo chage -M 90 -m 7 username
    ```
* **Set a warning period (10 days):**
    ```bash
    sudo chage -W 10 username
    ```
* **Force a password change on next login:**
    ```bash
    sudo chage -d 0 username
    ```

### B. Global Defaults (`/etc/login.defs`)
To set the policy for **all future users** created on the system, edit the login definition file.

1. Open the file: `sudo nano /etc/login.defs`
2. Locate and modify these variables:
    * `PASS_MAX_DAYS  90`
    * `PASS_MIN_DAYS  7`
    * `PASS_WARN_AGE  14`

> **Warning:** These changes are not retroactive. Existing users will keep their current settings.

### C. Using the `passwd` Command
A simpler, though less granular, alternative to `chage`.

* **Expire a password now:** `sudo passwd -e username`
* **Set max life:** `sudo passwd -x 90 username`

---

## 4. Advanced Enforcement (PAM)

Password aging only forces a *change*; it doesn't stop a user from switching back to their old password immediately. To prevent this, we use **PAM (Pluggable Authentication Modules)**.

Edit `/etc/pam.d/common-password` and add the `remember` option:

```text
password  sufficient  pam_unix.so obscure sha512 remember=5