---
Tags: 
Created: 2023-12-10 01:54:45
---
(Links:: [[Operating Systems]])

File systems are a way to *organize* and *store* information. They provide an abstraction over storage devices, and organize files in directories. File systems are structured as follows: ![[FS overview.png|500]]

# Files
Files can either be accessed sequentially or randomly. Files usually also have different types including: regular files, directories, soft links, special files (device files, metadata files). From the POV of the operating system, files are just a stream of bytes, which only get meaning from the user's program.

> [!question] When does the OS itself care about the contents of a file?

Different file systems have different limitations for file names, like the file *extensions*, the file *name length* and *special characters*. File systems can sometimes even be case sensitive, allowing you to store to files with the same name, but a character capitalized. 
On top of that, files can have certain attributes:

| Attribute          | Meaning                                               |
| ------------------ | ----------------------------------------------------- |
| Protection         | Who can access the file and in what way               |
| Password           | Password needed to access the file                    |
| Creator            | ID of the person who created the file                 |
| Owner              | Current owner                                         |
| Read-only flag     | 0 for read/write; 1 for read only                     |
| Hidden flag        | 0 for normal; 1 for do not display in listings        |
| System flag        | 0 for normal files; 1 for system file                 |
| Archive flag       | 0 for has been backed up; 1 for needs to be backed up |
| ASCII/binary flag  | 0 for ASCII file; 1 for binary file                   |
| Random access flag | 0 for sequential access only; 1 for random access     |

# Directories
These are data structures organizing and maintaining information about files. They are normally just stored as file entries with some special attributes set.
In the beginning we only had the root directory, where all files were stored. Nowadays we have hierarchical systems where we can have subdirectories.
# File System Implementation
The VFS is used as an interface between use programs and the underlying file systems. Programs only see a single file system.
![[Virtual File Systems.png|500]]

## How to store files
When we have a disk, it is sorted into *partitions*. At the beginning of the disk we have the **Master Boot Record**, used to boot the computer. After that comes the partition table, with the starting and ending addresses of each partition.
To store files, we use fundamental blocks (usually 4KB in size), where multiple of them can make up a single file. The blocks of a file need to be kept in a list of sorts. We usually use one of three strategies:
- Linked list
- File allocation table
- I-nodes

Using a linked list is pretty straight forward. Each file block has metadata with a pointer to the next block. This is *bad for random access*, and it also causes the stored data to *no long be a power-of-two* which is inefficient.
![[Storing a file as a linked list of disk blocks.png|500]]

We can remove this drawback by storing the pointers in a separate table. This however does not scale well, and requires a pre-allocated table for every block on the disk which can get large quickly.
![[Linked list allocation using a file allocation table in main memory.png|300]]

With **I-nodes**, we store a list of block addresses per file. For larger files, one i-node can point to additional blocks containing more data block addresses.
![[UNIX i-node.png|600]]
## How to implement directories
Directories can be stores as a fixed size structure with entries and their disk address and attributes. They can also be stored as a list with pointers to the i-nodes of those entries.
> [!example]- FAT12/FAT16 Layout
> ![[FAT12 Layout.png|500]]
> - Reserved later used for FS information
> - FAT `#1/#2`: preallocated File Allocation Tables
> - Preallocated root directory
> - Clusters represent addressable blocks: FAT contains chains indexed by starting cluster

On FAT12/16, a directory entry had the following format:
![[Directory entry.png|700]]
Filenames had the 8.3 format, and long file name entries were able to use multiple entries for a single file by setting some attribute data. There are however other methods of storing longer file names, by either having a *file entry length* storing also the length of the file name, or a *file name pointer*.
![[Handle long file names.png|500]]
On UNIX, each directory entry stored only the name (14 bytes) and the i-node number (2 bytes). All attributes are stored in the i-node.
These i-nodes are stored in a dedicated area after the boot block and before the data area.
Files can also be shared between different users. In this case we look at two different types of linking:
- Hard links reference a shared file (i-node)
	– The file is removed only with no hard links left
	– Space-efficient (only 1 directory entry per hard link)
	– Good to manage shared files across owners
- Soft (or symbolic) links reference a file name
	– Removing the file renders its soft links invalid
	– Less space-efficient (need 1 i-node per soft link)
	– More flexible (can reference file names across FSes)

## How to manage disk space
> [!question]+ How big should our block size be?
> - The larger the block size, the faster the data rate, but also potentially lower the disk space utilization and the more fragmentation:
> ![[Block size impact.png|500]]

If we want to create a new file, we need to keep track of the free blocks. We can do this  the same way we did with memory, by:
1. storing the free list on a linked list
2. using a bitmap
## How to ensure file system performance
Using caches, we can minimize the time that we go to disk. There are two types of cache:
- Buffer cache: Cache disk blocks in RAM
- Page cache: Cache VFS pages (before going to driver). Often there is the same data in the buffer cache, so the OS may merge them

We look at two types of caching:
- Write through caching: Write to cache and then directly to disk
- Periodic syncing: Write to cache and only write to disk when we evict it -> less overhead (if something goes wrong, the data on the disk is not updated)

Once again we can also use *disk read-ahead* to predict which block we may have to read.
> [!question] Why is yanking out a USB stick considered "unsafe"?

On older HDD disks, an arm is used to read data off of a disk. We can speed up the process of reading from the disk, by allocating files contiguously. We can also store i-nodes in different groups instead of at the start, so that seeking for the associated blocks does not take as long (on average it would be half the number of cylinders)
![[Spread i-nodes over disk.png|600]]

We need to optimize for smaller writes to disk, for this we use a log-structured file system. Instead of directly writing the changes to an i-node and the data block, we keep a queue of changes (segments). These segments can contain any sort of change. The segments are then flushed periodically to disk, and new changes are added to the back. Since the newest changes aren't written to disk, we need to keep an i-node map of the newest version.

## How to ensure file system dependability
Lots of things can happen to file systems, for example the disk on which the FS is stored can *fail*. Sometimes metadata is written inconsistently to disk. **Software bugs** can also occur resulting in data loss. Very often, errors occur from the *user*. Lastly, a computer can be stolen.
These issues can be mitigated by making backups. We consider different properties of backups:
- Incremental vs. full: Copy over entire disk every time or just changes
- Online vs. offline: Can we make a backup when we are still running?
- Physical vs. logical: Make a copy of every block vs. backup a single directory with files recursively
  ![[A file system to be dumped.png|500]]
- Compressed vs. uncompressed: compressing can take some time
- Local vs. remote: Store data on the same machine or maybe in the cloud

Nowadays, we usually use logical backup methods such as incremental logical backup.
The algorithm operates in four phases: 
- Phase 1 marks all modified files and directories in a bitmap. 
- Phase 2 unmarks directories that have no modified files or directories in them or under them. 
- Phase 3 scans the i-nodes in numerical order and dumps all the directories marked for dumping. 
- Phase 4 dumps the files marked in the bitmap. 

This process allows for the restoration of a specific file or directory upon request and ensures that the necessary directories and files are present for a complete restore, even if they have not been modified.

If we want to secure our backups we use RAID (Redundant Array of Independent Disks).
- RAID 0 & 2: Expand file system over multiple disks (+storage)
- RAID 1: Duplicate file system over disks (+redundancy)
- RAID 3-6: Store parity information (+storage, +redundancy)

> [!question] Does RAID help with the following threats:
> - Physical failure
> - Computer catching fire or getting stolen
> - power failure
> - Bug in driver
> - User error

When errors occur to the file system itself, we can use programs like `fsck` to make a *consistency check* to find errors in the file system metadata:
- Corrupt values
- Used blocks also marked as free
- Blocks not marked as free nor used
- Blocks being used multiple times

If a crash occurs, and we were in the middle of an operation, replaying this operation might lead to errors. For this reason we use "logs" for crash recovery. Each step in the operation should be **idempotent**, meaning if we do them again, it leads to the same state. After a crash we simply **replay the operations stored in the log**. This supports *multiple* arbitrary crashes.

Lastly we might want to delete files securely or encrypt sensitive data. Securely deleting data on a disk is notoriously **not easy**. We can physically destroy the disk, but this might not be a viable solution. We can instead overwrite the data with zeros, but this might not be enough, which is why we use do **multiple overwrites**.

---
References: [[Chapter 04 - MOS FileSystems.pptx.pdf]]