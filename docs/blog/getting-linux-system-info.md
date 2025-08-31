# Getting Linux System Info

Some commands to run to get different system information about a Linux system:

```bash
# To get Linux version, which Distro running, and CPU architecture
uname -a

# "List hardware" lists hardware and information about hardware (using sudo gives more information) 
sudo lshw

# "Firmware Update Manager" can show devices and their firmware version
fwupdmgr get-devices

# If running Ubuntu, you can get reportable system information
ubuntu-report show

# DMI (Desktop Management Interface) / SMBIOS (System Management Basic Input-Output System) information
sudo dmidecode
# Use -t option for selecting different "types" of information, e.g.
sudo dmidecode -t memory
```
