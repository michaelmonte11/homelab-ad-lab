# homelab-ad-lab

Samba Active Directory and self-hosted services on a repurposed 2015 MacBook Air.

A hands-on IT and cybersecurity lab built step by step on old hardware: Linux administration, networking, firewalls, directory services, and documentation. Everything runs on a home network only. Nothing is exposed to the internet.

> All user accounts in this lab are fictional.

## Hardware and constraints

- MacBook Air (2015) running Ubuntu 24.04 LTS
- ~3.7 GiB RAM, ~110 GB SSD, wired Ethernet through a USB adapter
- The limits shape every decision: lightweight software, careful storage planning, and heavy installs saved for last

## What's built

- **Host health check** after several power outages: boot history, kernel log, SMART status on the SSD, and filesystem state
- **Storage:** extended the LVM volume online to use the full SSD
- **Firewall:** UFW with default-deny inbound; every service limited to the home subnet
- **Offline library:** Kiwix serving Wikipedia (mini edition, sized to fit the disk) and WikiMed, with downloads verified by checksum
- **Active Directory:** Samba domain controller with Kerberos and DNS
  - Company-style structure: OUs for IT, HR, Sales, Servers, Workstations, and ServiceAccounts
  - Role-based groups and a dedicated service account for a future ticketing system
  - Password policy with minimum length, complexity, and account lockout
  - Offline domain backup, copied to a separate machine

## Troubleshooting notes

- **Hostname resolution:** Samba needs the server's name to resolve to its LAN address, not loopback, so I corrected `/etc/hosts` before provisioning.
- **Port 53 conflict:** Ubuntu's built-in resolver holds port 53, which Samba's DNS needs. I redirected `resolv.conf` first and then disabled the stub listener, so name lookups never broke.
- **Jellyfin install corruption:** the installer failed with decompression and checksum errors. The package had passed apt's verification, and the failure point moved between attempts, which suggests hardware instability under load. Next step is a RAM test before relying on this machine for heavy installs.

## Security notes

- Default-deny firewall; SSH and web services reachable only from the home network
- Domain backups contain password hashes and Kerberos keys, so they are never committed to this repo (see `.gitignore`)
- Lab-only choices are documented in the changelog (for example, non-expiring lab accounts)

## Not done yet

- SSH key-only login (password authentication is still enabled)
- RAM test, a clean reboot test of the domain controller, and a backup restore test
- Static IP and start-at-boot for Kiwix
- Docker, monitoring, offline maps, and a help-desk ticketing system tied to the domain

## Documentation

See [`docs/CHANGELOG.md`](docs/CHANGELOG.md) for dated notes on every change.

## License

MIT
