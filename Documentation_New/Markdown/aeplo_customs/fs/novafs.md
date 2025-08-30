# The Nova Filesystem
---------------------

NovaFS is a modern experimental filesystem forked for Linux.  
The core design idea is simplicity with room for growth — think of it as a foundation where we can explore new layouts, optimizations, and potentially re-think how filesystems interact with modern storage devices.  

Unlike the “big and mature” filesystems like XFS or ext4, Nova is kept lean at first, but the target is **performance, modularity, and experimentation**. NovaFS aims to support journaling, extended attributes, and flexible inode structures.  

Just like any filesystem, it also introduces its own inode type system, allocation strategy, and metadata handling — but we’ll document and explain every step right here in this repo.  
No need for external docs, though of course kernel.org’s docs are always worth checking out.  

---

## Location in the Kernel Source Tree
When you head into the Kernel source tree, under `fs/nova`, you’ll find the entire Nova source code.  
Over time, we’ll go file by file and annotate the codebase with clear comments, so you can actually follow along without needing to read obscure papers or reverse-engineer decades of history.  

For example:  

- **`fs/nova/nova_linux.h`** → This is where Nova defines its kernel-specific types.  
  One of the most important declarations is the **inode type**.  

---

## What is an inode?
Like in other filesystems, the **inode** is a data structure that describes metadata for a file or directory.  
It doesn’t store the filename or actual data; instead it holds:  

- **File Type** → regular file, directory, symlink, device, etc.  
- **Permissions** → read/write/execute bits.  
- **Ownership** → user ID + group ID.  
- **Size** → current size of the file.  
- **Timestamps** → created, modified, accessed.  
- **Pointers** → references to the actual storage blocks where the file’s contents live.  

Each file or directory gets a **unique inode** in NovaFS.  

---

## Inode Types in NovaFS
Just like in XFS or ext4, Nova defines different object types for inodes. Some common ones are:  

- `Regular File` → A standard file storing data.  
- `Directory` → Holds other files or directories.  
- `Symbolic Link` → Reference pointing somewhere else.  
- `Block Device` → For devices like `/dev/sda`.  
- `Character Device` → For things like `/dev/tty`.  
- `Socket` → For inter-process or network communication.  
- `FIFO / Pipe` → For process-to-process streams.  

To see inode types on your Linux system right now, run:  

```bash
ls -l
```

That first character tells you the inode type.

Why care? Because the inode type is how the kernel decides what you can do with an object.
For instance, you can’t cd into a plain file, but you can into a directory.

# SideNote:

NovaFS is experimental.
