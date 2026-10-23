# Ubuntu Server Troubleshooting Portfolio

A practical Linux systems-administration lab documenting investigation, remediation, and verification on **Ubuntu Server 26.04.1 LTS** running in Oracle VirtualBox.

## Environment

- **OS:** Ubuntu Server 26.04.1 LTS
- **Hostname:** `server1`
- **Lab account:** `samuel`
- **Network interface:** `enp0s3`
- **Observed IPv4 address:** `10.0.2.15/24` (VirtualBox NAT)

## Scenarios

### 1. File permissions and access control — completed

- Worked with `/srv/company/Payroll/report.txt`.
- Configured/verified `root:Payroll` ownership and mode `0660` (`-rw-rw----`).
- Verified Payroll group access, including reading and appending a test entry; Management access was denied as intended.
- Inspected group/user identities and directory/file permissions.

### 2. systemd service failure — completed

- Investigated `lab-demo.service` failing with `status=203/EXEC`.
- Journal output identified an invalid/nonexistent `ExecStart` executable path.
- Corrected the one-shot action to `/usr/bin/touch /tmp/lab-demo-success`, reloaded systemd, and verified successful execution using the marker file.
- Note: `inactive (dead)` can be expected after a successful `Type=oneshot` unit completes.

### 3. Disk usage and log investigation — completed controlled exercise

- Used `df -h` and `du` to inspect filesystem and log-directory usage.
- Created a controlled 200 MB test file under `/var/log/lab-demo`, which raised root filesystem usage from about 55% to 57%.
- Removed the test file and directory, then verified usage returned to about 55% with roughly 4.7 GB available.
- This was a controlled simulation, not a real disk-full outage.

### 4. SSH diagnostics and access verification — diagnostic exercise

- Verified port 22 listeners on IPv4 and IPv6.
- Observed `ssh.socket` enabled/listening and accounted for socket activation when `ssh.service` appeared inactive before a connection.
- Confirmed the VM address and successfully tested `ssh samuel@localhost`; authentication/session logs supported the result.
- No SSH repair was needed. This is documented as diagnostics and verification, not a repaired outage.

## Evidence and report

- `Ubuntu_Linux_Troubleshooting_Portfolio.pdf` — formatted report with embedded terminal screenshots.
- `evidence/` — selected screenshots from the submitted capture PDF, excluding unrelated Windows-only content.

## Useful commands

```bash
# Network and SSH
ip -br addr
ip route
ss -lntp
sudo sshd -t

# systemd and logs
systemctl status lab-demo.service --no-pager
sudo journalctl -u lab-demo.service -n 50 --no-pager
systemctl cat lab-demo.service

# Storage
df -h
df -i
sudo du -xhd1 /var/log 2>/dev/null | sort -h
sudo journalctl --disk-usage

# Permissions
id Payroll
getent group Payroll
ls -ld /srv/company/Payroll
ls -l /srv/company/Payroll/report.txt
```

## Scope and limitations

- The APT dependency exercise was started but not completed and is intentionally not listed as a finished scenario.
- The supplied screenshot pack included a Windows command-prompt image unrelated to this Linux portfolio; it was excluded.
- The lab was performed in a virtual machine. It demonstrates practical troubleshooting workflow but should not be represented as production incident experience.

## Skills demonstrated

Linux CLI • file permissions and groups • systemd and journald • filesystem capacity analysis • log investigation • SSH/socket diagnostics • evidence-based troubleshooting • post-change verification
