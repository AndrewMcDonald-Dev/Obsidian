Introduced with Windows Server 2021 with the intent to be the next generation file system after [[NTFS]]. The key improvements are the file systems ability to self-repair, along with abstraction or virtualization between physical disks and logical volumes.

ReFS includes automatic integrity checking and data scrubbing, elimination of the need for running chkdsk, protection against data degradation, built-in handling of hard disk drive failure and redundancy, integration of [[RAID]] functionality, a switch to copy/allocate on write for data and metadata updates, handling of very long paths and filenames, and storage virtualization and pooling, including almost arbitrarily sized logical volumes.