# Failure modes
*    Cosmic ray or other radiation flips a bit on disk, transitory flip in RAM, ...
*    Permanent computer hardware failure: Disk, RAID Controller, Other (CPU/RAM/Motherboard corruption could all be fatal/unrecoverable)
*    Network hardware failure: Landslide, lightning strike, power surge, fire, mass theft
*    Compromised software (OS/Driver/Software bugs might not be super likely to corrupt multiple systems in the same way, but ransomware sure might)

# Redundancy modes
*    RAID 1/5 (still vulnerable to a single hardware failure corrupting all disks)
*    Multiple copies on a partition (still vulnerable to a filesystem driver bug corrupting structure, also does nothing on deduplicating filesystems)
*    Multiple copies on a disk/raid, different partitions (still vulnerable to disk/raid controller driver bugs, but different partitions could have different filesystems)
*    Multiple copies on a computer, different disks/raid controllers (still vulnerable to a single hardware failure corrupting "everything", malware, etc.)
*    Multiple copies on a network, different computers (still vulnerable to a single power surge, fire, etc.)
*    Multiple copies at different geographical locations (still vulnerable to single-ecosystem bugs, global solar flares, etc.)
