# Changelog

Lab notes for a home server on a repurposed 2015 MacBook Air (Ubuntu 24.04 LTS, ~3.7 GiB RAM, wired Ethernet via USB adapter). Network addresses are generalized (192.168.x.x). All user accounts are fictional.

## 2026-10-03

### Domain controller (Samba Active Directory)
- Fixed the server's hostname resolution: replaced the loopback entry in /etc/hosts with the LAN address and fully qualified name (Samba requires this).
- Opened the AD ports in UFW, limited to the home subnet.
- Freed port 53: pointed /etc/resolv.conf at the upstream resolver, then disabled the systemd-resolved stub listener (in that order, so name lookups never broke).
- Installed Samba and provisioned the domain HOMELAB.INTERNAL (NetBIOS: HOMELAB, internal DNS backend, router as forwarder).
- Verified: DNS SRV records, Kerberos login (kinit/klist), SMB shares (sysvol, netlogon).
- Built a company-style structure: OUs for IT, HR, Sales, Servers, Workstations, and ServiceAccounts; role-based groups; fictional test users; a dedicated service account for a future ticketing system.
- Tightened the password policy (minimum length, complexity, account lockout). Administrator and lab accounts are set to never expire (lab-only decision).
- Took an offline domain backup, locked down its permissions, verified the archive lists correctly, and copied it to a separate machine. Restore test still to do.

### Storage
- Ran the SMART check on the SSD (PASSED, no reallocated or pending sectors) and a filesystem state check (clean).
- Extended the LVM volume from ~55 GiB to ~110 GiB online using the unallocated space in the volume group.

### Troubleshooting: Jellyfin install failures
- The installer failed with zstd decompression errors ("data corruption detected", then "restored data doesn't match checksum").
- Observation: apt verified the fresh download, and the failure point moved between attempts. That pattern points at hardware instability under load (RAM first) and not at a bad package.
- Cleared the package cache and retried; the install completed on the next attempt. RAM test (memtester) still to do before relying on the machine for heavy installs.

## 2026-10-02

- Installed Ubuntu 24.04 LTS and checked the system after several power outages (journal, boot history, mounted filesystem state).
- Enabled UFW with default-deny inbound; SSH and the Kiwix web service allowed only from the home subnet.
- Downloaded and checksum-verified offline content: Wikipedia (mini edition, chosen to fit the disk) and WikiMed. Served with kiwix-serve, tested through an SSH tunnel first, then bound to the LAN address.
- Decided against TLP (battery tool, not useful for a plugged-in server) to keep the baseline clean.

## Still to do
- SSH key-only login (password authentication still on)
- RAM test and a clean reboot test of the domain controller
- Restore test of the domain backup
- Static IP on the server and Kiwix start-at-boot
- Docker, offline maps, arcade (chess/Snake), monitoring
- Help-desk ticketing system integrated with the domain
