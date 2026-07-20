# rhel9-stig Audit changelog


## Based on STIG V2R8 - 2026 benchmark_v2r8 cycle

V2R8 benchmark bump (446 -> 446 rules; updates-only, no add/remove)
8 goss files moved cat_2/RHEL-09-NNxxxx/ subdirs -> cat_1/ flat for severity-bumped rules (215100, 215105, 255064, 255065, 255070, 255075, 671020, 672050) with Cat: 2 -> Cat: 1 in goss metadata
46 Rule_ID metadata lines updated to V2R8 SV-* values (27 expected from V2R7 -> V2R8 XCCDF + 19 pre-existing audit-repo drifts surfaced)
5 Vul_ID base corrections for rules with wrong V-* in audit metadata: RHEL-09-214015 (V-257826 -> V-257820), RHEL-09-231070 (V-257854 -> V-257855), RHEL-09-232210 (V-257923 -> V-257922), RHEL-09-271090 (V-258029 -> V-258028), RHEL-09-654097 (V-274878 -> V-279936)
vars/STIG.yml benchmark_version bumped v2r7 -> v2r8
run_audit.sh BENCHMARK_VER bumped v2r7 -> v2r8 (with v prefix preserved per RHEL convention, opposite of Ubuntu)
README banner updated V2R7 -> V2R8
fixed RHEL-09-251045 goss toggle gate: was `rhel_09_251040`, corrected to `rhel_09_251045` (selective `--vars` runs of rule 251045 silently escaped under the wrong toggle)
fixed RHEL-09-231190 goss `Cat:` metadata: 2 -> 1 (file is in cat_1/ and rule is HIGH severity; metadata drift pre-existed V2R8 cycle)
fixed RHEL-09-653120 goss YAML document marker: added blank line after `---` to match 445/446 sibling convention

July 2026 community issue fixes.
fixed RHEL-09-231105 goss /boot/efi nosuid check: corrected the inverted `grep -v nosuid` and the malformed test bracket so a /boot/efi mounted with nosuid passes instead of always returning Investigate (addresses #14; thank you @mbc3)
fixed RHEL-09-252040 goss NetworkManager DNS-mode regex to accept none, default, or systemd-resolved per the STIG (was none only), and corrected the malformed CCI-00366 to CCI-000366 (addresses #15; thank you @mbc3)
fixed RHEL-09-252020 goss chrony check: dropped the `.mil` restriction so any configured timeserver with maxpoll 1-16 passes, matching the STIG and the remediation role default servers; also tightened the maxpoll bound with a word boundary so values greater than 16 correctly fail (the prior `([1-9]|1[0-6])` matched the leading digit of 17+) (addresses #16; thank you @mbc3)
fixed RHEL-09-651035 goss: relaxed the over-strict aide ruleset regex to verify `xattrs` (the full-string match failed the remediation role aide.conf), corrected the title from "Access Control Lists (ACLs)" to "extended attributes", and renamed the check to aide_all_xattrs to match SV-258139 (addresses #13; thank you @mbc3)
relaxed the same over-strict aide regex on RHEL-09-651030 (verifies `acl`) and RHEL-09-651020 (verifies `sha512`), which carried the identical brittle pattern
corrected 8 malformed CCI metadata values missing leading zeros to their 6-digit XCCDF values: RHEL-09-252035, RHEL-09-252045, RHEL-09-252050, RHEL-09-411065, RHEL-09-411070 (CCI-000366), RHEL-09-411040 (CCI-000016), RHEL-09-411045 (CCI-000764), RHEL-09-672020 (CCI-003123)
reconciled two touched rules to the V2R8 XCCDF: RHEL-09-252020 added CCI-004923 and CCI-004926, and RHEL-09-411070 Vul_ID corrected V-258052 to V-258053

RHEL-09-611180: goss now checks the pcscd socket (`pcscd.socket`) instead of the pcscd service, per the V2R8 XCCDF check-content (`systemctl is-active pcscd.socket`). Title left as the DISA-verbatim "pcscd service". Mirrors the RHEL10-STIG-Audit RHEL-10-200611 fix.
run_audit.sh: read only the first line of `goss -v` (awk NR==1) so a multi-line goss banner cannot corrupt the parsed version; fixed two message typos ("needs to run", "does not meet minimum")

Full goss metadata reconciliation to the V2R8 XCCDF (80 rule files). Coverage 446/446, all Cat/severity already correct.
reconciled 2 stale Rule_ID revisions: RHEL-09-431015 (r958944 -> r1045159) and RHEL-09-653090 (r1101918 -> r1155630)
corrected 9 Vul_ID values: RHEL-09-212040, RHEL-09-214020, RHEL-09-232050, RHEL-09-252075, RHEL-09-433016, RHEL-09-654210, and fixed the SV- prefix on RHEL-09-215015, RHEL-09-215060, RHEL-09-654097 (were SV-, now V-)
reconciled 70 CCI sets to the XCCDF: added CCIs DISA introduced in later releases (for example CCI-003992, CCI-004066, CCI-004062, CCI-004046, CCI-004923/004926, CCI-004895, CCI-003938) to the rules that lacked them, and corrected wrong/typo CCIs (RHEL-09-651010/651015 CCI-001774 -> CCI-001744, RHEL-09-255010/255015 CCI-002322 -> CCI-002422, RHEL-09-271020/271025/271035 CCI-001985 -> CCI-001958, RHEL-09-232035 CCI-001439 -> CCI-001493, RHEL-09-251035 CCI-000366 -> CCI-000382, RHEL-09-412040 -> CCI-000054, RHEL-09-611145 CCI-002048 -> CCI-002038)

## Based on STIG V2R7 - May updates
Alignment
missing rule added
company naming
CONTRIBUTING.rst rebranded to Ansible-Lockdown Projects
fixed 7-digit STIG_ID typos in RHEL-09-211030 and RHEL-09-211040 goss tests
fixed STIG_ID metadata in RHEL-09-271095 goss (was incorrectly set to RHEL-09-271085)
fixed 7-digit .Vars typos and comamnd typo in RHEL-09-271100 and RHEL-09-271110 goss tests
fixed Rule_ID prefix (V- to SV-) and updated to V2R7 revision in RHEL-09-213100 goss
run_audit.sh: drop fragile `grep -w` from VERSION_ID detection (lesson #19) and add BENCHMARK_OS fallback when detection produces empty result (lesson #42)
Goss test titles aligned verbatim to V2R7 XCCDF for 12 controls where the audit title was cross-pasted, stale from a prior benchmark version, or contained a literal SV-ID: 213025 (kernel kexec->kernel pointer addresses), 215101 (SV-ID->Postfix package), 251045 conf (TCP syncookies->BPF JIT hardening), 271065 (15->10 minutes), 412035 (15->10 minutes), 412045 (concurrent sessions->log username on unsuccessful logon), 412070 (per-user umask->system default profile), 431016 (RHEL 8->RHEL 9), 432035 (privilege elevation->su command), 611040 (pam_faillock->password complexity in password-auth), 611045 (pam_faillock->password complexity in system-auth), 653015 (audit package->audit service enabled)
RHEL-09-271065 goss test: three bug fixes - (1) path appended .d suffix (was /etc/dconf/db/local/ would never exist; rem writes to local.d/), (2) section-header regex was a malformed character class /^\/[org\/...\]/ that matched literal "/" + one-of-set; corrected to /^\[org\/gnome\/desktop\/session\]$/, (3) content regex extended to accept the uint32 prefix produced by the rem and tightened max from 900 to 600 per XCCDF "greater than 600 is a finding".
RHEL-09-214025 goss title: added trailing period to align verbatim with V2R7 XCCDF.


## 2.7.0 Based on STIG V2R7 05 January 2026

Cat I
- 211010 - RuleID updated
- 211045 - Rewritten to use drop in file replacement
- 212020 - RuleID updated
- 214025 - RuleID updated
- 215060 - RuleID updated
- 671010 - RuleID updated
- 672020 - RuleID updated

Cat II

- 213010 - moved to drop in file
- 213015 - moved to drop in file
- 213020 - moved to drop in file
- 213025 - moved to drop in file
- 213030 - moved to drop in file
- 213035 - moved to drop in file
- 213070 - moved to drop in file
- 213075 - moved to drop in file
- 213080 - moved to drop in file
- 213085 - wont run if 213040 is true
- 213090 - wont run if 213040 is true
- 213095 - wont run if 213040 is true
- 213100 - wont run if 213040 is true
- 213105 - moved to drop in file
- 214030
- 215035 - no longer rsh-server now disable epel - new variable option
- 215045
- 215101
- 231105
- 231110
- 231115
- 231120
- 231200 - excluded vfat filesystem mount from search
- 232040 - Added group and change mode to symbolic
- 232240 - changed to capture any system user greater than UID 999
- 252040 - Extended variable option for NetworkManager DNS options default none
- 251035 - moved to drop in file
- 253010 - moved to drop in file
- 253015 - moved to drop in file
- 253020 - moved to drop in file
- 253025 - moved to drop in file
- 253030 - moved to drop in file
- 253040 - moved to drop in file
- 253045 - moved to drop in file
- 253050 - moved to drop in file
- 253055 - moved to drop in file
- 253060 - moved to drop in file
- 253070 - moved to drop in file
- 253075 - moved to drop in file
- 254010 - moved to drop in file
- 254105 - moved to drop in file
- 254020 - moved to drop in file
- 254025 - moved to drop in file
- 254030 - moved to drop in file
- 254035 - moved to drop in file
- 254040 - moved to drop in file
- 255100
- 255115 - added owner and group
- 255130
- 271065 - updated value to 10mins from 15min
- 411115 - Removed
- 412075 - Removed
- 412080 - changed timeout to 10mins
- 431016 ****NEEDS LOOKING AT*****
- 432025
- 611160
- 611170
- 611195 - rewrite - drop in file
- 611200 - rewrite - drop in file
- 611190 - rewritten to have check no longer manual
- 652025 - Improved test
- 652055 - rewritten and rainer script option added
- 653040
- 653090
- 653110
- 654101
- 654015
- 654020
- 654025
- 654065
- 654070
- 654075
- 654080
- 654096 - now 654097
- 654205
- 654210
- 654260 - removed

## 2.5.0 Based on STIG V2R5 07 August 2025

- Removed Requirements
  - RHEL-09-255055
  - RHEL-09-255060
  - RHEL-09-653115
  - RHEL-09-672025

- Added Requirements
  - RHEL-09-654096 - New rule to audit

- RuleID update for all listed
  - RHEL-09-212020
  - RHEL-09-213010
  - RHEL-09-213015
  - RHEL-09-213020
  - RHEL-09-213025
  - RHEL-09-213030
  - RHEL-09-213035
  - RHEL-09-213040
  - RHEL-09-213070
  - RHEL-09-213075
  - RHEL-09-213080
  - RHEL-09-213105
  - RHEL-09-231140
  - RHEL-09-232104
  - RHEL-09-232245
  - RHEL-09-411040
  - RHEL-09-412035
  - RHEL-09-251045
  - RHEL-09-253010
  - RHEL-09-253015
  - RHEL-09-253020
  - RHEL-09-253025
  - RHEL-09-253030
  - RHEL-09-253035
  - RHEL-09-253040
  - RHEL-09-253045
  - RHEL-09-253050
  - RHEL-09-253055
  - RHEL-09-253060
  - RHEL-09-253065
  - RHEL-09-253075
  - RHEL-09-254010
  - RHEL-09-254015
  - RHEL-09-254020
  - RHEL-09-254025
  - RHEL-09-254030
  - RHEL-09-254035
  - RHEL-09-254040
  - RHEL-09-215015
  - RHEL-09-215060
  - RHEL-09-215105
  - RHEL-09-231115
  - RHEL-09-232020
  - RHEL-09-232180
  - RHEL-09-232185
  - RHEL-09-232200
  - RHEL-09-232205
  - RHEL-09-251020
  - RHEL-09-251035
  - RHEL-09-252065
  - RHEL-09-432025
  - RHEL-09-432030
  - RHEL-09-611085
  - RHEL-09-611160
  - RHEL-09-611195
  - RHEL-09-611200
  - RHEL-09-651010
  - RHEL-09-651025
  - RHEL-09-652010
  - RHEL-09-652055
  - RHEL-09-653035
  - RHEL-09-653090
  - RHEL-09-653120
  - RHEL-09-654010
  - RHEL-09-654015
  - RHEL-09-654020
  - RHEL-09-654025
  - RHEL-09-654065
  - RHEL-09-654070
  - RHEL-09-654075
  - RHEL-09-654080
  - RHEL-09-654205
  - RHEL-09-654210
  - RHEL-09-654096
  - RHEL-09-654220
  - RHEL-09-672020

## V2r4

- RuleID for all listed
  - RHEL-09-212020 - ID
  - RHEL-09-212045 - title and requirements
  - RHEL-09-215060 - ID
  - RHEL-09-215101 - New control to install postfix
  - RHEL-09-232040 - ID and updated
  - RHEL-09-232200 - check files only
  - RHEL-09-232205 - ID
  - RHEL-09-255045 - ID
  - RHEL-09-255105 - All config files
  - RHEL-09-255110 - All config files
  - RHEL-09-255115 - title and requirement
  - RHEL-09-411045 - ID
  - RHEL-09-431016 - New control selinux
  - RHEL-09-611205 - rule removed
  - RHEL-09-232265 - rule removed
  - RHEL-09-654025 - ID updated
  - RHEL-09-671015 - ID updated

## V2r3

- RuleID Updates
- CCI Updates
- package removals/additions now dont skip if package present or not but run through giving correct state
- Upgraded several control to disruption_high
- authselect updates for faillock related controls
- var dictionaries renamed to allow easier overridding of variables
- RHEL-09-672010 - Becomes RHEL-09-215100
- RHEL-09-672020 - moved to CAT1 and approach changed to remediate inline with documentation
- RHEL-09-672030 - removed
- RHEL-09-171011 - Added
- RHEL-09-232103 - Added
- RHEL-09-232104 - Added
- RHEL-09-251025 - Added firewalld reload as issues seen for new connections
- RHEL-09-255064 - Added
- RHEL-09-255070 - Added
- RHEL-09-433016 - Added fapolicyd ading rules and testing - new var rhel9stig_allow_fapolicy_updates
- RHEL-09-610205 - title update
- RHEL-09-652035 - removed
- RHEL-09-653110 - added audit.rules
- RHEL-09-653130 - moved to 653xxx as 652035 no longer present
- RHEL-09-672035 - removed
- RHEL-09-672040 - removed
- RHEL-09-672045 - moved to 215105
