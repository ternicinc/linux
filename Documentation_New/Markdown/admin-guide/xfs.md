# The SGI XFS Filesystem
-----------------------

XFS is an high performance journaling filesystem. It's muti-threaded, can support large files and large filesystems.
It also supports extended attibutes, variable block sizes, and it's extent based. It also makes extensive use of BTrees to aid both performance and scalability.

We will maintain all documentation here, no need for any external platform. Regardless, it may be recomended to check out the official documentation on Kernel.org. (https://xfs.wiki.kernel.org/).

----

When you head over to the Kernel source tree, and head to `fs/xfs`, you are able to find the entire xfs source code. Which will will completly explain. We will also optimize comments within the actual
source-code. for clearity.

`fs/xfs/xfs_linux.h` = The `xfs_linux.h` file includes the kernel-specific type declarations, got XFS. You're able to see this within the file itself.
One of those declaratations is the `inode` type. The `inode` essentially is an index node, which is a data-structure that stores metadata about a file or directory. Yet, it does **not** include the
file's name or its actual corresponding data; it contains things like:

- File Type: regular file, directory, symlink.
- Permissions: Read/write/execute.
- Owner: UserID, and groupID.
- File Size: Size of the file.
- Timestamps: Created, modified, accessed.
- Pointers: To the actual data blocks on the disk.

Each file or directory has a unique inode in the filesystem.

The `inode` type, referenced as `xfs_ino_t` in `fs/xfs/xfs_linux.h` essentially means what type of object the inode represents, some common types are:

- `Regular File` - A standard file storing data.
- `directory` - A folder containing files or other dirs.
- `Symbolic Link` - A pointer to another file or directory.
- `Block Device` - Represents a device that handle's data in blocks. Think of `/dev/sda`.

> To see you're inode-types on Linux, you can use a command like `ls -l`. The first character in the permission string indicates the type:

```

-rw-r--r--  file.txt       # regular file (-)
drwxr-xr-x  mydir          # directory (d)
lrwxrwxrwx  link -> file   # symlink (l)

```
The inode types matters for a few reason: It determins how the Operating System will interact with the object and which operations are allowed, for example, you can't `cd` into a file.

---

**Side Note** Whenever Aeplo starts it's development and fork of the kernel, the chance is relativly high that we will implement another type of filesystem, custom made. More information regrading
that will be available soon.
