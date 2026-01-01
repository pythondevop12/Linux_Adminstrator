# Systemd Deep Dive: Mastering Units, Timers, and Performance

Systemd has evolved into the standard management suite for Linux, extending far beyond simple service initiation to provide a unified framework for task scheduling and system auditing.

---

## 1. Writing Custom Unit Files
Custom units allow administrators to define exactly how applications interact with the OS.

* **Configuration Path:** Custom units should be placed in `/etc/systemd/system/` to override package defaults and persist through updates.
* **Section Hierarchy:**
    * **[Unit]:** Defines metadata and dependencies (e.g., `After=network.target`).
    * **[Service]:** Specifies the execution logic, including `ExecStart`, `User`, `WorkingDirectory`, and `Restart` policies.
    * **[Install]:** Defines the "Target" or hook point for the service (e.g., `WantedBy=multi-user.target`).
* **Dependency Management:** Use `Wants=` for non-critical dependencies and `Requires=` for mandatory ones.



---

## 2. Managing Systemd Timers (The Modern Cron)
Timers are systemd unit files (`.timer`) that trigger `.service` files, offering a more robust alternative to the legacy `cron` utility.

* **Dual-Unit Requirement:** A timer requires both a `.timer` file (to set the schedule) and a matching `.service` file (to execute the task).
* **Timer Types:**
    * **Monotonic Timers:** Trigger relative to an event (e.g., `OnBootSec=10min` or `OnUnitActiveSec=1h`).
    * **Realtime Timers:** Trigger based on calendar events (e.g., `OnCalendar=*-*-* 02:00:00`).
* **Key Advantages:**
    * **Persistence:** `Persistent=true` ensures a missed job runs immediately after the next boot.
    * **Journaling:** All execution logs are automatically captured and searchable via `journalctl`.
    * **Resource Control:** Services triggered by timers can be restricted using Cgroups (CPU/RAM limits).

---

## 3. Analyzing Boot Speed with systemd-analyze
Systemd provides a built-in suite of performance auditing tools to identify and resolve boot bottlenecks.

* **Total Duration:** The `systemd-analyze` command provides a high-level summary of time spent in the Kernel, Initrd, and Userspace.
* **Blame Analysis:** Running `systemd-analyze blame` lists all active units ordered by the time they took to initialize, pinpointing the slowest services.
* **Critical Chain:** `systemd-analyze critical-chain` visualizes the dependency tree of time-critical units, showing exactly which service is delaying the system reaching its final target.
* **Graphical Plotting:** `systemd-analyze plot > boot.svg` generates a detailed vector graphic timeline of the entire boot sequence for visual debugging.



---

## 4. Operational Summary Table

| Feature | Key Command | Benefit |
| :--- | :--- | :--- |
| **Service Creation** | `systemctl daemon-reload` | Refreshes systemd to recognize new or edited units. |
| **Task Scheduling** | `systemctl list-timers` | Displays all active schedules and their next execution time. |
| **Performance Audit** | `systemd-analyze blame` | Identifies specific services causing boot delays. |
| **Log Inspection** | `journalctl -u <unit>` | Provides a unified view of service history and errors. |

---

## 5. Walkthrough: Converting Cron to Systemd Timer
To move a job from cron to systemd, follow this two-file logic:

### Step 1: Create the Service (`/etc/systemd/system/mytask.service`)
```ini
[Unit]
Description=My Script Execution Service

[Service]
Type=oneshot
ExecStart=/usr/bin/bash /path/to/script.sh
User=root
```

### Step 2: Create the Timer (`/etc/systemd/system/mytask.timer`)
```ini
[Unit]
Description=Run my script every day at 3 AM

[Timer]
OnCalendar=*-*-* 03:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

### Step 3: Activation
```bash
systemctl daemon-reload
systemctl enable --now mytask.timer
```
---