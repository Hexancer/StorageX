#### 1. The difference between read/write and pread/pwrite

```c
#include <unistd.h>

ssize_t read(int fd, void *buf, size_t count);
ssize_t write(int fd, const void *buf, size_t count);

ssize_t pread(int fd, void *buf, size_t count, off_t offset);
ssize_t pwrite(int fd, const void *buf, size_t count, off_t offset);
```

```c
#include <sys/types.h>
#include <unistd.h>

off_t lseek(int fd, off_t offset, int whence);
/* whence
* SEEK_SET: The offset is set to offset bytes
* SEEK_CUR: The offset is set to its current location plus offset bytes
* SEEK_END: The offset is set to the size of the filed plus offset bytes
* since version 3.1, Linux supports additional whence
* SEEK_DATA: Adjust the file offset to the next location in the file
*greater than or equal to offset containing data. If offset points to
*data, then the file offset is set to offset.
* SEEK_HOLE: Adjust the file offset to next hole in the file greater than
*or equal to offset. If offsets points into the middle of a hole, then the
*file offset is set to offset. If there is no hole past offset, then the 
*file offset is adjusted to the end of the file(i.e., there is an implicit
*hole at the end of any file).
*/
```

**A hole is a gap within the file for which there are no allocated data blocks.**  



E.g. a 3KB file contains a 1KB data block followed by a 1KB hole followed by another 1KB data block. **When reading over a hole, zeroes will be returned.**



进程打开的每个文件都有一个当前文件指针(current file pointer/file offset)指示下次读取和写入的位置。

> ***Pread()* works just like *read()* but reads from  the specified position in the file without modifying the file pointer.** 
> 
> This atomicity of pread  enables processes or threads that share file descriptors to read from a shared file at a particular offset without using a locking mechanism that would be necessary to achieve the same result in separate lseek and read system calls. 
> 
> Atomicity is required as the file pointer is shared and  one thread might move the pointer using lseek after another process completes an lseek but prior to the read.
> 
> --- Stackoverflow-[what is the difference between read and pread in unix](https://unix.stackexchange.com/questions/166569/is-partition-table-type-loop-a-good-or-bad-idea-on-btrfs)

lseek + write, pwrite 类似

---

#### 2. cacheline padding/cacheline alignment

In the context of struct in programming, cacheline padding is a technique used to add additional padding bytes or dummy variables to a struct to align its size with the cacheline size.

##### Reasons

- Preventing false sharing
False sharing occurs when two or more variables that are frequently accessed by different threads or CPU cores are located in the same cache line. When one thread modifies a variable, it may invalidate the cache line and force other threads to reload the entire cache line, even if they are only accessing a different variable within the same cache line.

> 一言以蔽之，结构中常用的变量独占一个cache行


This technique should be use wisely and only if necessary, too much padding to a struct waste memory and negatively impact cache efficiency.

#### 3. bcache(block cache) 

> Cache in the Linux kernel's block layer.

Bcache 允许更快的存储设备如SSDs可以作为慢速存储设备HDDs的cache。结合SSDs昂贵的速度和传统HDDs便宜的存储容量，这有效的创建了hybrid volumes并提供性能提升。

