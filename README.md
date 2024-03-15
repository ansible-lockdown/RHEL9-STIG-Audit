# RHEL9 Goss config

## Note: Ansible playbook currently in subscription only release

## Overview

based on STIG v1r2

Ability to audit a system using a lightweight binary to check the current state.

This is:

- very small 11MB
- lightweight
- self contained

It works using a set of configuration files and directories to audit STIG of RHEL 9 servers. These files/directories correlate to the STIG Level and STIG_ID

Tested on

- RHEL9
- Rocky9
- Alma-Linux9

feedback on any differences between OSs please raise an issue

## Join us

On our [Discord Server](https://discord.io/ansible-lockdown) to ask questions, discuss features, or just chat with other Ansible-Lockdown users

## Requirements

You must have [goss](https://github.com/goss-org/goss/) available to your host you would like to test.

You must have sudo/root access to the system as some commands require privilege information.

Assuming you have already clone this repository you can run goss from where you wish.

Please refer to the audit documentation for usage.

- [Audit Documents](https://ansible-lockdown.readthedocs.io/en/latest/audit/getting-started-audit.html)

This also works alongside the [Ansible Lockdown RHEL8-STIG role](https://github.com/ansible-lockdown/RHEL8-STIG)

Which will:

- install
- audit
- remediate
- audit

## variables

These are found in vars/STIG.yml
Please refer to the file for all options and their meanings

STIG listed variable for every control/benchmark can be turned on/off or section

### The variable files

In this case installed or skipped using the standard name for a package to be installed or _skip to skip a test.

### Extra settings

Some sections can have several options in that case the skip flag maybe passed to the test or exact details relating to your requirements
e.g.

- rhel9stig_use_gui
- rhel9stig_is_router
- rhel9_stig_nameservers:
  - 8.8.8.8
  - 9.9.9.9

## Examples

- full check

```sh

# {{path to your goss binary}} --vars {{ path to the vars file }} -g {{path to your clone of this repo }}/goss.yml v

```

- example:

```sh
# /usr/local/bin/goss --vars ../vars/stig.yml -g /home/bolly/rh7_cis_goss/goss.yml validate
......FF....FF................FF...F..FF.............F........................FSSSS.............FS.F.F.F.F.........FFFFF....

Failures/Skipped:

Title: CAT_2 | RHEL-09-040641 | Must ignore Internet Protocol version 4 (IPv4) Internet Control Message Protocol (ICMP) redirect messages from being accepted.
KernelParam: net.ipv4.conf.all.accept_redirects: value:
Expected
    <string>: 1
to equal
    <string>: 0

Title: CAT_2 | RHEL-09-021000 | Must prevent files with the setuid and setgid bit set from being executed on file systems that are used with removable media.
Mount: /mnt: exists:
Expected
    <bool>: false
to equal
    <bool>: true

< ---------cut ------- >

Title: CAT_2 | RHEL-09-010280 | Must be configured so that passwords are a minimum of 15 characters in length.
File: /etc/security/pwquality.conf: contains: patterns not found: [/^minlen = 15/]

Title: CAT_2 | RHEL-09-040500 | Must for networked systems, synchronize clocks with a server that is synchronized to one of the redundant United States Naval Observatory (USNO) time servers, a time server designated for the appropriate DoD network (NIPRNet/SIPRNet), and/or the Global Positioning System (GPS).
File: /etc/chrony.conf: contains: patterns not found: [/^server.*maxpoll 10/]

Title: CAT_2 | RHEL-09-010310 | Must disable account identifiers (individuals, groups, roles, and devices) if the password expires.
File: /etc/default/useradd: contains: patterns not found: [/^INACTIVE=0/]

Total Duration: 40.673s
Count: 1000, Failed: 51, Skipped: 6
```

- running a particular section of tests

```sh
# /usr/local/bin/goss -g /home/bolly/rh9_cis_goss/section_1/cis_1.1/cis_1.1.22.yml  validate
............

Total Duration: 0.033s
Count: 12, Failed: 0, Skipped: 0
```

- changing the output

```sh
# /usr/local/bin/goss -g /home/bolly/rh9_stig_goss/Cat_2/RHEL-09-010030.yml  validate -f documentation
goss -g Cat_2/RHEL-09-020240.yml  --vars vars/stig.yml v -f documentation
Title: CAT_2 | RHEL-09-020240 | Must define default permissions for all authenticated users in such a way that the user can only read and modify their own files.
File: /etc/login.defs: exists: matches expectation: [true]
File: /etc/login.defs: mode: matches expectation: ["0644"]
File: /etc/login.defs: contains: patterns not found: [/^UMASK 077]

Failures/Skipped:

Title: CAT_2 | RHEL-09-020240 | Must define default permissions for all authenticated users in such a way that the user can only read and modify their own files.
File: /etc/login.defs: contains: patterns not found: [/^UMASK 077]

Total Duration: 0.000s
Count: 3, Failed: 1, Skipped: 0
```

## Helpful

If you want to run locally and se easy putput of failures, the following command helps

```sh
$ sudo ./run_audit.sh -f documentation

## Pre-Checks Start

OK - Audit binary /usr/local/bin/goss is available
OK - Goss is installed and version is ok (0.4.4 >= 0.4.0)
OK - /opt/RHEL9-STIG-Audit/goss.yml is available

## Pre-checks Successful

#############
Audit Started
#############

Total Duration: 40.673s
Count: 1000, Failed: 51, Skipped: 6
Completed file can be found at /opt/audit_rocky9-bios-STIG-RHEL9_1700237280.documentation

###############
Audit Completed
###############

$ awk 'f;/Failures/{f=1}' /opt/audit_rocky9-bios-STIG-RHEL9_1700237280.documentation | grep -w "Title" | cut -d: -f2 | sort
RHEL-09-211025 | RHEL 9 must implement the Endpoint Security for Linux Threat Prevention tool. | Package
 RHEL-09-211025 | RHEL 9 must implement the Endpoint Security for Linux Threat Prevention tool. | Service
 RHEL-09-231090 | RHEL 9 must prevent files with the setuid and setgid bit set from being executed on file systems that are used with removable media.
 RHEL-09-231105 | RHEL 9 must prevent files with the setuid and setgid bit set from being executed on the /boot/efi directory.
 RHEL-09-231190 | RHEL 9 local disk partitions must implement cryptographic mechanisms to prevent unauthorized disclosure or modification of all information that requires at rest protection.
 RHEL-09-231190 | RHEL 9 local disk partitions must implement cryptographic mechanisms to prevent unauthorized disclosure or modification of all information that requires at rest protection. | disks encrypted
 RHEL-09-232240 | All RHEL 9 world-writable directories must be owned by root, sys, bin, or an application user.
 RHEL-09-232245 | A sticky bit must be set on all RHEL 9 public directories.
 RHEL-09-232250 | All RHEL 9 local files and directories must have a valid group owner.
 RHEL-09-232255 | All RHEL 9 local files and directories must have a valid owner.
 RHEL-09-232260 | RHEL 9 must be configured so that all system device files are correctly labeled to prevent unauthorized modification.
 RHEL-09-232265 | RHEL 9 /etc/crontab file must have mode 0600.
 RHEL-09-232270 | RHEL 9 /etc/crontab file must have mode 0600.
 RHEL-09-671010 | RHEL 9 must enable FIPS mode.

```

## Known Issues

STIG Control

- RHEL-09-211035 rngd service - this is replaced if running FIPS see https://bugzilla.redhat.com/show_bug.cgi?id=2208049

## further information

- [goss documentation](https://github.com/goss-org/goss/blob/master/docs/manual.md#patterns)
- [STIG standards](https://public.cyber.mil/stigs/)

## Feedback required

- If using nftables or iptables rather than firewalld
