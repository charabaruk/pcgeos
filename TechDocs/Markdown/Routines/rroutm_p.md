## 3.5 Routines M-P
----------
#### MakeWWFixed()
    WWFixed MakeWWFixed(number);

This macro casts a floating-point or integer number to a WWFixed value.

**Include:** geos.h

----------
#### malloc()
    void    * malloc(
            size_t      blockSize);         /* # of bytes to allocate*/

The **malloc()** family of routines is provided for Standard C compatibility. If 
a geode needs a small amount of fixed memory, it can call one of the routines. 
The kernel will allocate a fixed block to satisfy the geode's **malloc()** requests; 
it will allocate memory from this block. When the block is filled, it will 
allocate another fixed malloc-block. When all the memory in the block is 
freed, the memory manager will automatically free the block.

When a geode calls **malloc()**, a section of memory of the size specified will be 
allocated out of its malloc-block, and the address of the start of the memory 
will be returned. The memory will *not* be zero-initialized. If the request 
cannot be satisfied, **malloc** will return a null pointer. The memory is 
guaranteed not to be moved until it is freed (with **free()**) or resized (with 
**realloc()**). When GEOS shuts down, all fixed blocks are freed, and any 
memory allocated with **malloc()** is lost.

Using too many fixed blocks degrades the memory manager's performance, 
slowing the whole system. For this reason, applications should not use 
**malloc**-family routines if they can possibly be avoided. They are provided 
only to simplify porting of existing programs; however, applications should 
make every effort to use the GEOS memory management and LMem routines 
instead. If you must use the **malloc**-family routines, use them sparingly, and 
free the memory as quickly as possible.

**Tips and Tricks:** You can allocate memory in another geode's malloc-block by calling 
**GeoMalloc()**. However, that block will be freed when the other geode exits.

**Warnings:** All memory allocated with **malloc()** is freed when GEOS shuts down.

**Include:** stdlib.h

**See Also:** calloc(), free(), GeoMalloc(), realloc()

----------
#### ManufacturerFromFormatID
    word    ManufacturerFromFormatID(id);
            ClipboardItemFormatID id;

This macro extracts the word-sized manufacturer ID (of type 
**ManufacturerIDs**) from a **ClipboardInfoFormatID** argument.

----------
#### MemAlloc()
    MemHandle MemAlloc(
            word            byteSize,       /* Size of block in bytes */
            HeapFlags       hfFlags,        /* Type of block */
            HeapAllocFlags  haFlags);       /* How to allocate block */

This routine allocates a global memory block and creates an entry for it in the 
global handle table. The properties of the block are determined by the 
**HeapFlags** record passed; the way the block should be allocated is 
determined by the **HeapAllocFlags** record. Both sets of flags are described 
below. The routine returns the block's handle. If it could not allocate the 
block, it returns a null handle. The block allocated may be larger than the 
size requested, as the block size is rounded up to the next even paragraph 
(one paragraph equals sixteen bytes).

**HeapFlags** are stored in the block's handle table entry. They can be 
retrieved with the routine **MemGetInfo()**; some of them can be changed 
with the routine **MemModifyFlags()**. The following flags are available:

HF_FIXED  
The block will not move from its place in the global heap until 
it is freed. If this flag is off, the memory manager may move the 
block while it is unlocked. If the flag is on, the block may not be 
locked, and HF_DISCARDABLE and HF_SWAPABLE must be off.

HF_SHARABLE  
The block may be locked by threads belonging to geodes other 
than the block's owner.

HF_DISCARDABLE  
The block may be discarded when unlocked.

HF_SWAPABLE  
The block may be swapped to extended/expanded memory or to 
the disk swap space when it is unlocked.

HF_LMEM  
The block contains a local memory heap. This flag is set 
automatically by **LMemInitHeap()** and **VMAllocLMem()**; 
applications should not need to set this flag.

HF_DISCARDED  
The memory manager turns this bit on when it discards a 
block. The bit is turned off when the block is reallocated.

HF_SWAPPED  
The memory manager turns this bit on when it swaps a block 
to extended/expanded memory or to the disk swap space. It 
turns the bit off when it swaps the block back into the global 
heap.

**HeapAllocFlags** indicate how the block should be allocated and initialized. 
They are not stored and can not be retrieved. Some of the flags can be passed 
with **MemReAlloc()**. The following flags are available:

HAF_ZERO_INIT  
The memory manager should initialize the block to null bytes. 
This flag may be passed to **MemReAlloc()** to cause new 
memory to be zero-initialized.

HAF_LOCK  
The memory manager should lock the block after allocating it. 
To get the block's address, call **MemDeref()**. This flag may be 
passed to **MemReAlloc()**.

HAF_NO_ERR  
The memory manager should not return errors. If it cannot 
allocate block, GEOS will crash. Use of this flag is strongly 
discouraged. This flag may be passed to **MemReAlloc()**.

HAF_UI  
If both HAF_OBJECT_RESOURCE and HAF_UI are set, this 
block will be run by the application's UI thread.

HAF_READ_ONLY  
The block's data will not be modified. Useful for the debugger.

HAF_OBJECT_RESOURCE  
This block will be an object block.

HAF_CODE  
This block contains executable code.

HAF_CONFORMING  
If the block contains code, the code may be run by a less 
privileged entity. If the block contains data, the data may be 
accessed or altered by a less privileged entity.

**Include:** heap.h

**See Also:** MemAllocSetOwner(), MemReAlloc(), MemDeref()

----------
#### MemAllocLMem()
        MemHandle MemAllocLMem(
            LMemType    type,               /* type of LMem block */
            word        headerSize);        /* size of header structure */

This routine allocates and initializes a local memory block; it can be used to 
simplify this procedure from the two-step process of **MemAlloc()** followed by 
**LMemInitHeap()**. Pass an LMem type indicating what will be stored in the 
block, along with the size of the header structure to use. If the block is to have 
the standard header, pass zero in *headerSize*.

This routine returns the handle of the unlocked, newly allocated block. The 
block will contain two LMem handles and 64 bytes allocated for the LMem 
heap.

**Include:** lmem.h

**See Also:** LMemInitHeap()

----------
#### MemAllocSetOwner()
    MemHandle MemAllocSetOwner(
            GeodeHandle     owner,              /* Handle of block's owner */
            word            byteSize,           /* Size of block in bytes */
            HeapFlags       hfFlags,            /* Type of block */
            HeapAllocFlags  haFlags);           /* How to allocate block */

This routine is the same as **MemAlloc()** except that you can specify the 
owner of the global memory block created.

**Include:** heap.h

**See Also:** MemAlloc()

----------
#### MemDecRefCount()
    void    MemDecRefCount(
            MemHandle       mh);            /* handle of affected block */

This routine decrements the reference count of a global memory block (the 
reference count is stored in *HM_otherInfo*). If the reference count reaches 
zero, **MemDecRefCount()** will free the block.

**Warnings:** This routine assumes that a reference count is stored in *HM_otherInfo*. You 
may only use this routine if the block has had a reference count set up with 
**MemInitRefCount()**.

**Include:** heap.h

----------
#### MemDeref()
    void    * MemDeref(
            MemHandle   mh);    /* Handle of locked block to dereference */

This routine takes one argument, the handle of a global memory block; it 
returns the address of the block on the global heap. If the block has been 
discarded, or if the handle is not a memory handle, it returns a null pointer. 
It gets this information by reading the block's handle table entry; it does not 
need to actually access the block.

Note that if the handle is of an unlocked, moveable block, **MemDeref()** will 
return the block's address with out any warning; however, the address will 
be unreliable, since the memory manager can move the block at any time.

**Include:** heap.h

**Tips and Tricks:** This is very useful when you allocate a fixed or locked block, and need to get 
the block's address without calling **MemLock()**.

**Warnings:** This routine, if given an unlocked, moveable block, will return the pointer 
without a warning, even though that block may move at any time.

**See Also:** MemGetInfo(), MemModifyFlags()

----------
#### MemDowngradeExclLock()
    void    MemDowngradeExclLock(
            MemHandle       mh);        /* handle of affected block */

An application that has an exclusive lock on a block may downgrade it to a 
shared lock with this routine. It does not otherwise affect the block.

**Include:** heap.h

----------
#### MemFree()
    void    MemFree(
            MemHandle       mh);        /* handle of block to be freed */

This routine frees a global memory block. The block can be locked or 
unlocked. 

**Include:** heap.h

**Warnings:** The routine does not care whether other threads have locked the block. If you 
try to free a bad handle, routine may fatal-error.

----------
#### MemGetInfo()
    word    MemGetInfo( /* return value depends on flag passed */
            MemHandle       mh,     /* Handle of block to get info about */
            MemGetInfoType  info);  /* Type of information to get */

**MemGetInfo()** is a general-purpose routine for getting information about a 
global memory block. It gets the information by looking in the block's handle 
table entry; it does not need to access the actual block. It returns a single 
word of data; the meaning of that word depends on the value of the 
**MemGetInfoType** variable passed. The following types are available:

MGIT_SIZE  
Return value is size of the block in bytes. This may be larger 
than the size originally requested, as blocks are allocated along 
paragraph boundaries.

MGIT_FLAGS_AND_LOCK_COUNT  
The upper byte of the return value is the current **HeapFlags** 
record for the block. The lower byte is the number of locks 
currently on the block.

MGIT_OWNER_OR_VM_FILE_HANDLE  
If the block is part of a VM file, return value is the VM file 
handle. Otherwise, return value is the GeodeHandle of the 
owning thread.

MGIT_ADDRESS  
Return value is block's segment address on the global heap, or 
zero if block has been discarded. If block is unlocked and 
moveable, address may change without warning. Ordinarily, 
**MemDeref()** is preferable.

MGIT_OTHER_INFO  
Returns the value of the *HM_otherInfo* word. This word is used 
in different ways for different types of handles.

MGIT_EXEC_THREAD  
Returns the ThreadHandle of the thread executing this block, 
if any.

**Include:** heap.h

**Warnings:** If the handle is not a global memory block handle, results are unpredictable 
(the routine will read inappropriate data from the handle table entry).

**See Also:** MemDeref(), MemModifyFlags(), MemModifyOtherInfo(), 
HandleModifyOwner()

----------
#### MemIncRefCount()
    void    MemIncRefCount(
            MemHandle       mh);        /* handle of affected block */

This routine increments the reference count of a global memory block (the 
reference count is stored in *HM_otherInfo*).

**Warnings:** This routine assumes that a reference count is stored in *HM_otherInfo*. You 
may only use this routine if the block has had a reference count set up with 
**MemInitRefCount()**.

**Include:** heap.h

----------
#### MemInitRefCount()
    void    MemInitRefCount(
            MemHandle   mh,             /* handle of affected block */
            word        count);         /* initial reference count */

This routine sets up a reference count for the specified global memory block. 
The passed count is stored in the *HM_otherInfo* field of the block's 
handle-table entry.

**Warnings:** This routine overwrites the *HM_otherInfo* field. Since the semaphore 
routines (**HandleP()** and **HandleV()** and the routines which use them) use 
this field, you cannot use both the semaphore routines and the reference 
count routines on the same block.

**Include:** heap.h

----------
#### MemLock()
    void    * MemLock(
            MemHandle       mh);        /* Handle of block to lock */

This routine locks a global memory block on the global heap. If the block is 
swapped, the memory manager swaps it back into the global heap; it then 
increments the lock count (up to a maximum of 255). The block will not be 
moved, swapped, or discarded until the lock count reaches zero. This routine 
returns a pointer to the start of the block, or a null pointer if block has been 
discarded. To get the address of a block without locking it, use **MemDeref()**.

**Include:** heap.h

**Warnings:** If you try to lock a bad handle, the routine may fatal-error. This routine does 
not check for synchronization problems; if the block is used by several 
threads, you should use the synchronization routines.

**Never Use Situations:** Never lock a fixed block.

**See Also:** MemDeref()

----------
#### MemLockExcl()
    void    * MemLockExcl(
            MemHandle       mh);        /* Handle of block to grab */

If several different threads will be accessing the same global memory block, 
they should use data-access synchronization routines. **MemLockExcl()** 
belongs to one such set of routines. Often, several threads will need access to 
the same block; however, most of the time, they will not need to change the 
block. There is no synchronization problem if several threads read the same 
block at once, as long as none of them alters the block (by resizing it, writing 
to it, etc.) However, if a thread needs to change a block, no other thread 
should have access at that time.

The routines **MemLockExcl()**, **MemLockShared()**, 
**MemUnlockShared()**, and **MemUnlockExcl()** take advantage of this 
situation. They maintain a queue of threads requesting access to a block. 
When the block is not being used, they awaken the highest priority thread on 
the queue. If that thread requested exclusive access, the other threads sleep 
until it relinquishes access (via **MemUnlockExcl()**). If it requested shared 
access, the routines awaken every other thread which requested shared 
access; the other threads on the queue will sleep until every active thread 
relinquishes access (via **MemUnlockShared()**).

**MemLockExcl()** requests exclusive access to the block. If the block is not 
being accessed, the routine will grab exclusive access for the block, lock the 
block, and return the block's address. If the block is being accessed, the 
routine will sleep on the queue until it can get access; it will then awaken, 
lock the block, and return its address.

**Include:** heap.h

**Tips and Tricks:** You can find out if the block is being accessed by looking at the *HM_otherInfo* 
word (with **MemGetInfo()**). If *HM_otherInfo* equals one, the block is not 
grabbed; if it equals zero, it is grabbed, but no threads are queued; otherwise, 
it equals the handle of the first thread queued.

**Be Sure To:** Make sure that all routines accessing the block get access with 
**MemLockExcl()** or **MemLockShared()**. The routines use the block's 
*HM_otherInfo* word; you must not alter it. When you are done accessing the 
block, make sure to relinquish access by calling **MemUnlockExcl()**.

**Warnings:** If a thread calls **MemLockExcl()** when it already has shared or exclusive 
access, it will deadlock; it will sleep until access is relinquished, but it cannot 
relinquish access while it is sleeping. If you try to grab a block which is owned 
by a different geode and is non-sharable, the routine will fatal-error.

**Never Use Situations:** Never use **MemLockExcl()** or **MemLockShared()** on a fixed block. It will 
attempt to lock the block, and fixed blocks cannot be locked. Instead, use the 
**HandleP()** and **HandleV()** routines.

**See Also:** MemLockShared(), MemUnlockExcl(), MemUnlockShared()

----------
#### MemLockFixedOrMovable()
    void    * MemLockFixedOrMovable(
            void    * ptr);     /* virtual segment */

Given a virtual segment, this routine locks it (if it was movable). A virtual 
segment is an opaque pointer to a block that an application views as locked 
or fixed - the memory manager can actually swap locked or fixed blocks and 
will designate them as virtual segments.

**Include:** heap.h

----------
#### MemLockShared()
    void    * MemLockShared(
            MemHandle       mh);            /* Handle of block to grab */

**MemLockShared()** requests shared access to the passed block. If the block 
is not being accessed, or if it is held for shared access and the queue is empty, 
the routine gets access, locks the block, and returns the block's address. 
Otherwise it sleeps on the queue until the shared requests are awakened; it 
then locks the block and returns the block's address.

**Include:** heap.h

**Be Sure To:** Make sure that all routines accessing the block get access with 
**MemLockExcl()** or **MemLockShared()**. The routines use the block's 
*HM_otherInfo* word; you must not alter it. When you are done accessing the 
block, make sure to relinquish access by calling **MemUnlockExcl()**.

**Warnings:** If a thread calls **MemLockShared()** when it already has exclusive access, it 
will deadlock; it will sleep until access is relinquished, but it cannot 
relinquish access while it is sleeping. The thread must be careful not to take 
actions which could change the block, such as resizing it or writing to it. The 
routine will not enforce this. If you try to grab a block which is owned by a 
different geode and is non-sharable, the routine will fatal-error.

**Never Use Situations:** Never use **MemLockExcl()** or **MemLockShared()** on a fixed block. It will 
attempt to lock the block, and fixed blocks cannot be locked. Instead, use the 
**HandleP()** and **HandleV()** routines.

**See Also:** MemLockExcl(), MemUnlockExcl(), MemUnlockShared()

----------
#### MemModifyFlags()
    void    MemModifyFlags(
            MemHandle       mh,             /* Handle of block to modify */
            HeapFlags       bitsToSet,      /* HeapFlags to turn on */
            HeapFlags       bitsToClear);   /* HeapFlags to turn off */

**MemModifyFlags()** changes the **HeapFlags** record of the global memory 
block specified. Not all flags can be changed after the block is created; only 
the flags HF_SHARABLE, HF_DISCARDABLE, HF_SWAPABLE, and HF_LMEM 
can be changed.

The routine uses the handle table entry of the block specified; it does not need 
to look at the actual block. The routine performs normally whether or not the 
block is locked, fixed, or discarded.

**Include:** heap.h

**Warnings:** If the handle is not a global memory handle, results are unpredictable; the 
routine will change inappropriate bits of the handle table entry.

**See Also:** MemGetInfo(), HandleModifyOwner(), MemModifyOtherInfo()

----------
#### MemModifyOtherInfo()
    void    MemModifyOtherInfo(
            MemHandle   mh,             /* Handle of block to modify */
            word        otherInfo);     /* New value of HM_otherInfo word */

Use this routine to change the value of the global memory block's 
*HM_otherInfo* word. Some blocks need this word left alone; for example, 
data-access synchronization routines use this word.

**Include:** heap.h

**See Also:** MemGetInfo(), MemModifyFlags(), HandleModifyOwner()

----------
#### MemOwner()

GeodeHandle MemOwner(

    MemHandle       mh);            /* handle of block queried */

This routine returns the owning geode of the passed block. If the block 
belongs to a VM file, the owner of the VM file will be returned (unlike 
**MemInfo()**, which returns the VM file handle).

**Include:** heap.h

----------
#### MemPLock()
    void    * MemPLock(
            MemHandle       mh);        /* Handle of block to lock */

If several different threads will be accessing the same global memory block, 
they need to make sure their activities will not conflict. The way they do that 
is to use synchronization routines to get control of a block. **MemPLock()** is 
part of one set of synchronization routines.

If the threads are using the **MemPLock()** family, then whenever a thread 
needs access to the block in question, it can call **MemPLock()**. This routine 
calls **HandleP()** to get control of the block; it then locks the block and returns 
its address. If the block has been discarded, it grabs the block and returns a 
null pointer; you can then reallocate the block. When the thread is done with 
the block, it should release it with **MemUnlockV()**.

**Include:** heap.h

**Tips and Tricks:** You can find out if the block is being accessed by looking at the *HM_otherInfo* 
word (with **MemGetInfo()**). If *HM_otherInfo* equals one, the block is not 
grabbed; if it equals zero, it is grabbed, but no threads are queued; otherwise, 
it equals the handle of the first thread queued.

**Be Sure To:** Make sure that all threads accessing the block use **MemPLock()** and/or 
**HandleP()** to grab the block. These routines use the *HM_otherInfo* field of 
the block's handle table entry; do not alter this field. Release the block with 
**HandleV()** or **MemUnlockV()** when you are done with it.

**Warnings:** If a thread calls **MemPLock()** when it already has control of the block, it will 
deadlock. **MemThreadGrab()** avoids this conflict. If you try to grab a 
non-sharable block owned by another thread, **MemPLock()** will fatal-error.

**Never Use Situations:** Never use **MemPLock()** with a fixed block. It will try to lock the block, and 
fixed blocks cannot be locked. Instead, use **HandleP()**.

**See Also:** HandleP(), MemUnlockV(), HandleV()

----------
#### MemPtrToHandle()
    MemHandle MemPtrToHandle(
            void    * ptr);     /* pointer to locked block */

This routine returns the global handle of the locked block.

**Include:** heap.h

----------
#### MemReAlloc()
    MemHandle   MemReAlloc(
            MemHandle       mh,                 /* Handle of block */
            word            byteSize,           /* New size of the block */
            HeapAllocFlags  heapAllocFlags);    /* How to reallocate block */

This routine reallocates a global memory block. It can be used to resize a 
block; it can also be used to reallocate memory for a discarded block. Locked 
and fixed blocks can be reallocated; however, they may move on the global 
heap, so all pointers within the block must be adjusted. Note, however, that 
if the new size is smaller than the old size, the block is guaranteed not to 
move. The reallocated block may be larger than the size requested, as the 
block size is rounded up to the next even paragraph (one paragraph equals 
sixteen bytes).

The routine is passed a record of **HeapAllocFlags**. Only the flags 
HAF_ZERO_INIT, HAF_LOCK, and HAF_NO_ERR may be passed.

**Include:** heap.h

**Warnings:** If HAF_LOCK is passed, the lock count will be incremented even if the block 
is already locked by this thread. The routine does not care whether the block 
has been locked by another thread (possibly belonging to another geode); 
thus, if the block is being used by more than one thread, it is important to use 
the synchronization routines.

**See Also:** MemAlloc(), MemDeref()

----------
#### MemThreadGrab()
    void    * MemThreadGrab(
            MemHandle       mh);        /* Handle of block to grab */

**MemThreadGrab()** is used in conjunction with **MemThreadGrabNB()** 
and **MemThreadRelease()** to maintain data-access synchronization. If 
several threads will all have access to the same global memory block, they 
should use data-acess synchronization routines to make sure that their 
activities do not conflict. If a thread uses **MemThreadGrab()** and no other 
thread has grabbed the block in question, the routine will increment the 
"grab count," lock the block, and return its address. It can do this even if the 
calling thread has already grabbed the block. If another thread has grabbed 
the block, **MemThreadGrab()** will put the calling thread in a queue to get 
the block; the thread will sleep until it gets the block, then 
**MemThreadGrab()** will grab the block, lock it, and return its address.

If the block has been discarded, **MemThreadGrab()** grabs the block and 
returns a null pointer; you can then reallocate memory for the block.

**Include:** heap.h

**Be Sure To:** Make sure that all threads using the block use the **MemThread...()** routines 
to access it (not other data-acess synchronization routines). Do not change 
the *HM_otherInfo* word of the block's handle table entry (the routines use 
that word as a semaphore).

**Warnings:** If you try to grab a block which is owned by a different geode and is 
non-sharable, the routine will fatal-error.

**Never Use Situations:** Never use **MemThreadGrab()** with a fixed block. It will try to lock the 
block, and fixed blocks cannot be locked. If you need data-access 
synchronization for a fixed block, use the **HandleP()** and **HandleV()** 
routines.

**See Also:** MemThreadGrabNB(), MemThreadRelease()

----------
#### MemThreadGrabNB()
    void    * MemThreadGrabNB(
            MemHandle       mh);    /* handle of block to grab */

This is a data-synchronization routine to be used in conjunction with 
**MemThreadGrab()** and **MemThreadRelease()**. It is exactly the same as 
**MemThreadGrab()**, except that if it cannot grab the global memory block 
because another thread has it, the routine returns an error instead of 
blocking.

If successful, **MemThreadGrabNB()** returns a pointer to the block. If the 
block has been discarded, it grabs the block and returns a null pointer; you 
can then reallocate memory for the block. If the block has been grabbed by 
another thread, **MemThreadGrab()** returns the constant 
BLOCK_GRABBED.

**Include:** heap.h

**Tips and Tricks:** You can find out if the block is being accessed by looking at the *HM_otherInfo* 
word (with **MemGetInfo()**). If *HM_otherInfo* equals one, the block is not 
grabbed; if it equals zero, it is grabbed, but no threads are queued; otherwise, 
it equals the handle of the first thread queued.

**Be Sure To:** Make sure that all threads using the block use the **MemThread...()** routines 
to access the block (not other data-access synchronization routines). Do not 
change the *HM_otherInfo* word of the block's handle table entry (the routines 
use that word as a semaphore).

**Warnings:** If you try to grab a block that is owned by a different geode and is 
non-sharable, the routine will fatal-error.

**Never Use Situations:** Never use **MemThreadGrabNB()** with a fixed block. It will try to lock the 
block, and fixed blocks cannot be locked. If you need synchronization for a 
fixed block, use the **HandleP()** and **HandleV()** routines.

**See Also:** MemThreadGrab(), MemThreadRelease()

----------
#### MemThreadRelease()
    void    MemThreadRelease(
            MemHandle       mh);    /* handle of locked block to release */

Use this routine to release a global memory block which you have grabbed 
with **MemThreadGrab()** or **MemThreadGrabNB()**. The routine 
decrements the grab count; if the grab count reaches zero, the routine 
unlocks the block.

**Include:** heap.h

**Tips and Tricks:** You can find out if the block is being accessed by looking at the *HM_otherInfo* 
word (with **MemGetInfo()**). If *HM_otherInfo* equals one, the block is not 
grabbed; if it equals zero, it is grabbed, but no threads are queued; otherwise, 
it equals the handle of the first thread queued.

**Be Sure To:** Make sure that all threads using the block use the **MemThread...()** routines 
to access the block (not other data-access synchronization routines). Do not 
change the *HM_otherInfo* word of the block's handle table entry (the routines 
use that word as a semaphore). Make sure to release the block once for every 
time you grab it; the block is not unlocked until each of your grabs is released.

**Warnings:** If you try to release a block that you have not successfully grabbed, the 
routine will fatal-error.

**See Also:** MemThreadGrab(), MemThreadGrabNB()

----------
#### MemUnlock()
    void    MemUnlock(
            MemHandle       mh);        /* Handle of block to unlock */

This routine decrements the lock count of the indicated block. If the lock 
count reaches zero, the block becomes unlocked (it can be moved, swapped, 
or discarded). Do not try to unlock a block that has not been locked.

**Include:** heap.h

----------
#### MemUnlockExcl()
    void    MemUnlockExcl(
            memHandle       mh);        /* Handle of block to release */

If a thread has gained access to a block with **MemLockExcl()**, it should 
release the block as soon as it can. Until it does, no other thread can access 
the block for either shared or exclusive access. It can release the block by 
calling **MemUnlockExcl()**. This routine unlocks the block and releases the 
thread's access to it. If there is a queue for this block, the highest-priority 
thread waiting will be awakened, as described in **MemLockExcl()**.

**Include:** heap.h

**Tips and Tricks:** You can find out if the block is being accessed by looking at the *HM_otherInfo* 
word (with **MemGetInfo()**). If *HM_otherInfo* equals one, the block is not 
grabbed; if it equals zero, it is grabbed, but no threads are queued; otherwise, 
it equals the handle of the first thread queued.

**Be Sure To:** Make sure that all routines accessing the block get access with 
**MemLockExcl()** or **MemLockShared()**. The routines use the block's 
*HM_otherInfo* word; you must not alter it. Call this routine while the block is 
still locked; it will call **MemUnlock()** to unlock the block.

**Warnings:** If you call this routine on a block which you have not gained access to, it may 
fatal-error.

**See Also:** MemLockExcl(), MemLockShared(), MemUnlockShared()

----------
#### MemUnlockFixedOrMovable()
    void    MemUnlockFixedOrMovable(
            void    * ptr);     /* virtual segment */

This routine unlocks a previously locked, movable virtual segment. Do not 
call this routine with normal locked or fixed blocks; only call it for those 
blocks locked with **MemLockFixedOrMovable()**.

**Include:** heap.h

----------
#### MemUnlockShared()
    void    MemUnlockShared(
            MemHandle       mh);        /* Handle of block to release */

If a thread has gained access to a block with **MemLockShared()**, it should 
release the block as soon as it can. Until it does, no thread can be awakened 
from the queue. It can release the block by calling **MemUnlockShared()**. 
This routine calls **MemUnlock()**, decrementing the block's lock count; it 
then releases the thread's access to it. If no other thread is accessing the 
block and there is a queue for this block, the highest-priority thread waiting 
will be awakened, as described in **MemLockExcl()**.

**Include:** heap.h

**Tips and Tricks:** You can find out if the block is being accessed by looking at the *HM_otherInfo* 
word (with **MemGetInfo()**). If *HM_otherInfo* equals one, the block is not 
grabbed; if it equals zero, it is grabbed, but no threads are queued; otherwise, 
it equals the handle of the first thread queued.

**Be Sure To:** Make sure that all routines accessing the block get access with 
**MemLockExcl()** or **MemLockShared()**. These routines use the block's 
*HM_otherInfo* word; you must not alter it. Call this routine while the block is 
still locked; it will call **MemUnlock()** to unlock the block.

**Warnings:** If you call this routine on a block which you have not gained access to, it may 
fatal-error.

**See Also:** MemLockExcl(), MemLockShared(), MemUnlockExcl()

----------
#### MemUnlockV()
    void    MemUnlockV(
            MemHandle       mh);        /* Handle of block to release */

This routine unlocks a block with **MemUnlock()**, then releases its 
semaphore with **HandleV()**. Do not use this routine unless the block's 
semaphore was grabbed and the block locked (typically with the 
**MemPLock()** routine).

**Include:** heap.h

**Tips and Tricks:** You can find out if the block is being accessed by looking at the *HM_otherInfo* 
word (with **MemGetInfo()**). If *HM_otherInfo* equals one, the block is not 
grabbed; if it equals zero, it is grabbed, but no threads are queued; otherwise, 
it equals the handle of the first thread queued.

**Be Sure To:** Make sure that all threads accessing the block use **HandleP()** or 
**MemPLock()** to access the thread. These routines use the *HM_otherInfo* 
field of the handle table entry; do not alter this field.

**Warnings:** Do not use this on a block unless you have grabbed it. The routine does not 
check to see that you have grabbed the thread; it just clears the semaphore 
and returns.

**Never Use Situations:** Never use this routine to release a fixed block. It will try to unlock the block; 
fixed blocks cannot be locked or unlocked. Instead, call **HandleV()** directly.

**See Also:** MemPLock(), HandleP(), HandleV()

----------
#### MemUpgradeSharedLock()
    void    * MemUpgradeSharedLock(
            MemHandle       mh);        /* handle of locked block */

This routine upgrades a shared lock on the block to an exclusive lock, as if 
the caller had used **MemLockExcl()**. If other threads have access to the block, 
the caller will sleep in the access queue until it can gain exclusive access.

This routine returns the pointer of the locked block because, if the caller 
sleeps in the queue, the memory block could move between the call and the 
granting of access.

**Include:** heap.h

**See Also:** MemLockExcl(), MemLockShared(), MemDowngradeExclLock()

----------
#### MessageSetDestination()
    void    MessageSetDestination(
            EventHandle     event,      /* handle of the event to be modified */
            optr            dest);      /* new destination for the event */

This routine sets the destination of an event to the optr passed.

**Include:** object.h

----------
#### NameArrayAdd()
    word    NameArrayAdd(
            optr                arr,        /* optr of name array */
            const char          * nameToAdd,/* Name of new element */
            word                nameLength, /* Length of name; pass zero if
                                             * name string is null-terminated */
            NameArrayAddFlags   flags,      /* Described below */
            const void          * data);    /* Copy this data to new element */

This routine creates a new element in a name array, copying the passed name 
and data into the new element. If no element with the passed name exists, 
**NameArrayAdd()** will create the element and return its token. If an 
element with the same name already exists, the existing element's reference 
count will be incremented and its token will be returned. The routine takes 
the following arguments:

*array* - The optr of the name array.

*nameToAdd* - The name of the new element. This string may contain nulls.

*nameLength* - The length of the name string, in bytes. If you pass zero, 
**NameArrayAdd()** will assume the string is null-terminated.

*flags* - A record of **NameArrayAddFlags**, described below.

*data* - The data to copy into the new element.

**Structures:** The argument is passed a set of **NameArrayAddFlags**. Only one flag is 
currently defined:

NAAF_SET_DATA_ON_REPLACE  
If an element with the specified name exists and this flag is set, 
the data passed will be copied into the data area of the existing 
element. If this flag is not set, the existing element will not be 
changed.

**Warnings:** This routine may resize the name array; therefore, all pointers to the LMem 
heap are invalidated.

**Include:** chunkarr.h

----------
#### NameArrayAddHandles()
    dword   NameArrayAddHandles(
            MemHandle           mh,         /* Handle of LMem heap */
            ChunkHandle         arr,        /* Chunk handle of name array */
            const char *        nameToAdd,  /* Name of new element */
            word                nameLength, /* Length of name; pass zero if
                                             * name string is null-terminated */
            NameArrayAddFlags   flags,      /* Described below */
            const void *        data);      /* Copy this data to new element */

This routine is exactly like **NameArrayAdd()** above, except that the name 
array is specified by its global and chunk handles (instead of with an optr).

**Warnings:** This routine may resize the name array; therefore, all pointers to within the 
LMem heap are invalidated.

**Include:** chunkarr.h

----------
#### NameArrayChangeName()
    void    NameArrayChangeName(
            optr        array,          /* optr of name array */
            word        token,          /* Token of element to be changed */
            const char * newName,       /* New name for element */
            word        nameLength);    /* Length of name in bytes; pass
                                         * zero if name string is
                                         * null-terminated */

This routine changes the name of an element in a name array.

**Warnings:** If the new name is longer than the old, the chunk will be resized, invalidating 
all pointers to within the LMem heap.

**Include:** chunkarr.h

----------
#### NameArrayChangeNameHandles()
    dword   NameArrayChangeNameHandles(
            MemHandle       mh,             /* Handle of LMem heap */
            ChunkHandle     array,          /* Chunk handle of name array */
            word            token,          /* Token of element to be changed */
            const char *    newName,        /* New name for element */
            word            nameLength);    /* Length of name in bytes; pass
                                             * zero if name string is
                                             * null-terminated */

This routine is exactly like **NameArrayChangeName()** above, except that 
the name array is specified by its global and chunk handles (instead of with 
an optr).

**Warnings:** If the new name is longer than the old, the chunk will be resized, invalidating 
all pointers to within the LMem heap.

**Include:** chunkarr.h

----------
#### NameArrayCreate()
    ChunkHandle     NameArrayCreate(
            MemHandle   mh,             /* Global handle of LMem heap */
            word        dataSize,       /* Size of data section for
                                         * each element */
            word        headerSize);    /* Size of header; pass
                                         * zero for default header */

This routine creates a name array in the indicated LMem heap. It creates a 
**NameArrayHeader** structure at the head of a new chunk. If you want to 
leave extra space before the start of the array, you can pass a larger header 
size; if you want to use the standard header, pass a header size of zero.

You must specify the size of the data portion of each element when you create 
the array. The name portion will be variable sized.

**Include:** chunkarr.h

**Tips and Tricks:** If you want extra space after the **NameArrayHeader**, you may want to 
create your own header structure, the first element of which is a 
**NameArrayHeader**. You can pass the size of this header to 
**NameArrayCreate()** and access the data in your header via the structure 
fields.

**Be Sure To:** Lock the block on the global heap before calling this routine (unless it is 
fixed). If you pass a header size, make sure it is larger than 
**sizeof(NameArrayHeader)**.

**Include:** chunkarr.h

----------
#### NameArrayCreateAt()
    ChunkHandle     NameArrayCreateAt(
            optr    array,      /* Turn this chunk into a name array */
            word    dataSize,   /* Size of data section of each element */
            word    headerSize);/* Size of header; pass zero for default header */

This routine is just like **NameArrayCreate()** above, except that the element 
array is created in a pre-existing chunk. The contents of that chunk will be 
destroyed.

**Include:** chunkarr.h

**Warnings:** If the chunk isn't large enough, it will be resized. This will invalidate all 
pointers to chunks in that block.

**Include:** chunkarr.h

----------
#### NameArrayCreateAtHandles()
    ChunkHandle     NameArrayCreateAtHandles(
            MemHandle       mh,         /* Global handle of LMem heap */
            ChunkHandle     chunk,      /* the chunk for the array */
            word            dataSize,   /* Size of data for each element */
            word            headerSize);/* Size of header; pass
                                         * zero for default header */

This routine is exactly like **NameArrayCreateAt()** above, except that the 
name array is specified by its global and chunk handles (instead of with an 
optr).

**Include:** chunkarr.h

**Warnings:** If the chunk isn't large enough, it will be resized. This will invalidate all 
pointers to chunks in that block.

**Include:** chunkarr.h

----------
#### NameArrayFind()
    word    NameArrayFind(
            optr        array,          /* optr to name array */
            const char  * nameToFind,   /* Find element with this name */
            word        nameLength,     /* Pass zero if name string is
                                         * null-terminated */
            void *      returnData);    /* Copy data section into this
                                         * buffer */

This routine locates the element with the specified name. It returns the token 
of the element and copies its data section into the passed buffer. If there is no 
element with the specified name, the routine will return 
CA_NULL_ELEMENT. The routine takes the following arguments:

*array* - The optr of the name array.

*nameToAdd* - The name of the element to find. This string may contain nulls.

*nameLength* - The length of the name string, in bytes. If you pass zero, 
**NameArrayFind()** will assume the string is null-terminated.

*returnData* - The data section of the element is written to this buffer.

**Include:** chunkarr.h

**Warnings:** You must make sure the *returnData* buffer is large enough to hold an 
element's data portion; otherwise, data after the buffer will be overwritten.

----------
#### NameArrayFindHandles()
    word    NameArrayFindHandles(
            MemHandle       mh,             /* Handle of LMem heap */
            ChunkHandle     array,          /* Handle of name array */
            const char *    nameToFind,     /* Find element with this name */
            word            nameLength,     /* Pass zero if name string is
                                             * null-terminated */
            void *          returnData);    /* Copy data section into this
                                             * buffer */

This routine is exactly like **NameArrayFind()** above, except that the name 
array is specified by its global and chunk handles (instead of with an optr).

**Include:** chunkarr.h

----------
#### NEC()
    NEC(line)

This macro defines a line of code that will only be compiled into the 
*non*-error-checking version of the geode. The *line* parameter of the macro is 
the actual line of code. When the non-EC version of the program is compiled, 
the line will be treated as a normal line of code; when the EC version is 
compiled, the line will be ignored.

**Include:** ec.h

----------
#### ObjBlockGetOutput()
    optr    ObjBlockGetOutput(
            MemHandle   mh);        /* handle of the subject object block */

This routine returns the optr of the output destination object set for the 
specified object block. The output object is stored in the object block's header 
in the *OLMBH_output* field. If the block has no output set, NullOptr will be 
returned.

**Include:** object.h

**See Also:** ObjLMemBlockHeader

----------
#### ObjBlockSetOutput()
    void    ObjBlockSetOutput(
            MemHandle   mh,         /* handle of the subject object block */
            optr        o);         /* optr of the new output object */

This routine sets the *OLMBH_output* field in the specified object block's 
header. The optr passed in *o* will be set as the block's output.

**Include:** object.h

**See Also:** ObjLMemBlockHeader

----------
#### ObjCompAddChild()
    void    ObjCompAddChild(
            optr    obj,            /* optr of parent composite */
            optr    objToAdd,       /* optr of new child */
            word    flags,          /* CompChildFlags */
            word    masterOffset,   /* offset to master part */
            word    compOffset,     /* offset to comp field in master part */
            word    linkOffset);    /* offset to link field in master part */

This routine adds the given object to an object tree as the child of another 
specified object. It returns nothing. You will not likely want to use this 
routine; instead, you will probably use the messages listed below under "See 
Also." The parameters of this routine are

*obj* - The optr of the parent composite object. The parent must be a 
composite; if it is not, an error will result.

*objToAdd* - The optr of the child object. The child must have a link instance 
field (defined with **@link**).

*flags* - A record of **CompChildFlags**. These flags indicate whether 
the object should initially be marked dirty as well as where in 
the parent's child list the new child should be placed (first, 
second, last, etc.).

*masterOffset* - The offset within the parent's instance chunk to the master 
group's offset. (The value that would appear in the parent class' 
*Class_masterOffset* field in its **ClassStruct** structure.)

*compOffset* - The offset within the parent's instance chunk to the composite 
field.

*linkOffset* - The offset within the parent's instance chunk to the link field.

**Warnings:** This routine may resize and move LMem and Object blocks on the heap, 
thereby invalidating all segment addresses and pointers.

**Include:** metaC.goh

**See Also:** MSG_VIS_ADD_CHILD, MSG_GEN_ADD_CHILD

----------
#### ObjCompFindChildByNumber()
    optr    ObjCompFindChildByNumber(
            optr    obj,            /* parent's optr */
            word    childToFind,    /* zero-based child number */
            word    masterOffset,   /* offset to master part */
            word    compOffset,     /* offset to comp field in master part */
            word    linkOffset);    /* offset to link field in master part */

This routine returns the optr of the passed object's child; the child is specified 
based on its position in the object's child list. You will not often use this 
routine, but you will probably use the messages listed under "See Also" 
instead. The routine's parameters are listed below:

*obj* - The optr of the parent object.

*childToFind* - The zero-based number of the child to be found. For example, 
to return the first child's optr, pass zero or CCO_FIRST; to 
return the last child's optr, pass CCO_LAST.

*masterOffset* - The offset within the parent's instance chunk to the master 
group's offset. (The value that would appear in the parent class' 
*Class_masterOffset* field in its **ClassStruct** structure.)

*compOffset* - The offset within the parent's instance chunk to the composite 
field.

*linkOffset* - The offset within the parent's instance chunk to the link field.

**Include:** metaC.goh

**See Also:** MSG_GEN_FIND_CHILD, MSG_VIS_FIND_CHILD

----------
#### ObjCompFindChildByOptr()
    word    ObjCompFindChildByOptr(
            optr    obj,            /* parent's optr */
            optr    childToFind,    /* optr of child to find */
            word    masterOffset,   /* offset to master part */
            word    compOffset,     /* offset to comp field in master part */
            word    linkOffset);    /* offset to link field in master part */

This routine returns the zero-based child number of an object given its optr 
and its parent's optr. The returned number represents the child's position in 
its parent's child list. For example, a return value of zero indicates the object 
is the parent's first child. You will not likely use this routine; instead, you will 
probably use the messages shown below under "See Also."

The parameters for this routine are listed below:

*obj* - The optr of the parent object.

*childToFind* - The optr of the child whose number is to be returned. If the 
child is not found, the routine will return -1.

*masterOffset* - The offset within the parent's instance chunk to the master 
group's offset. (The value that would appear in the parent class' 
*Class_masterOffset* field in its **ClassStruct** structure.)

*compOffset* - The offset within the parent's instance chunk to the composite 
field.

*linkOffset* - The offset within the parent's instance chunk to the link field.

**Include:** - metaC.goh

**See Also:** MSG_GEN_FIND_CHILD, MSG_VIS_FIND_CHILD

----------
#### ObjCompMoveChild()
    void    ObjCompMoveChild(
            optr    obj,            /* parent's optr */
            optr    objToMove,      /* optr of child to move */
            word    flags,          /* CompChildFlags */
            word    masterOffset,   /* offset to master part */
            word    compOffset,     /* offset to comp field in master part */
            word    linkOffset);    /* offset to link field in master part */

This routine moves the specified child within its parent's child list. This 
routine will not move a child from one parent to another, but it can reorganize 
a parent's children. You will not likely use this routine, but you may often use 
the messages listed under "See Also" below.

The parameters of this routine are shown below:

*obj* - The optr of the parent object.

*objToMove* - The optr of the child to be moved. If the optr does not point to a 
valid child, behavior is undefined and an error is likely.

*flags* - A record of **CompChildFlags** indicating the new position of 
the child and whether the link should be marked dirty.

*masterOffset* - The offset within the parent's instance chunk to the master 
group's offset. (The value that would appear in the parent class' 
*Class_masterOffset* field in its **ClassStruct** structure.)

*compOffset* - The offset within the parent's instance chunk to the composite 
field.

*linkOffset* - The offset within the parent's instance chunk to the link field.

**Warnings:** This routine may cause LMem and/or Object Blocks to move or to shuffle 
their chunks, thereby invalidating any segment addresses or pointers.

**Include:** metaC.goh

**See Also:** MSG_GEN_MOVE_CHILD, MSG_VIS_MOVE_CHILD

----------
#### ObjCompProcessChildren()
    Boolean ObjCompProcessChildren(
            optr            obj,            /* parent's optr */
            optr            firstChild,     /* optr of first child to process */
            ObjCompCallType stdCallback,    /* standard callback type */
            void            * cbData,       /* data passed to callback */
            word            masterOffset,   /* offset to master part */
            word            compOffset,     * offset to comp field */
            word            linkOffset);    /* offset to link field */
            Boolean _pascal (*callback) (optr parent, optr child, void *cbData));

This routine performs a specific set of actions on all or some of an object's 
children. It is very rare that you will use this routine; typically, you should 
send a message to all of the parent's children. If, however, you use this 
routine, you must also write a callback routine that will be executed once for 
each affected child.

**ObjCompProcessChildren()** returns *true* (nonzero) only if it was stopped 
before all children had been processed. The only two ways this could be 
returned is if an error occurs or if your callback returns *true* when called.

The parameters for this routine are

*obj* - The optr of the composite whose children are to be processed.

*firstChild* - The optr of the first child to be processed. This routine will 
begin with the passed child and continue with all subsequent 
children. Pass the optr of the composite's first child - retrieved 
with the routine **ObjCompFindChildByNumber()** - to 
process all children.

*stdCallback* - A value of **ObjCompCallType** indicating how the data in the 
buffer pointed to by *cbData* will be passed to your callback 
routine. These values are detailed below.

*cbData* - A pointer to a buffer in which data can be passed to your 
callback routine. This buffer can be altered by your callback.

*masterOffset* - The offset within the parent's instance chunk to the master 
group's offset. (The value that would appear in the parent class' 
*Class_masterOffset* field in its **ClassStruct** structure.)

*compOffset* - The offset within the parent's instance chunk to the composite 
field.

*linkOffset* - The offset within the parent's instance chunk to the link field.

*callback* - A pointer to the actual callback routine that will be executed 
once for each child. The callback should be in your geode's fixed 
memory. The parameters and return values for the callback 
routine are given below.

The callback routine takes three parameters and returns a boolean value. It 
must be declared _pascal. The three parameters to the callback are listed 
below:

*parent* - The optr of the parent composite.

*child* - The optr of the current child being processed.

*cbData* - A pointer to the buffer passed by the original caller of 
**ObjCompProcessChildren()**. What is actually in this buffer 
may depend on the value in the original *sdtCallback* 
parameter; if the buffer is not saved and restored by 
**ObjCompProcessChildren()** between children, each child 
may receive data altered by the previous child.

The callback routine can access and alter the buffer pointed to by cbData, or 
it can query the child or do anything else with the exception of destroying the 
child. It should return a Boolean value: *true* if 
**ObjCompProcessChildren()** should be aborted, *false* if it should not.

The values you can pass to **ObjCompProcessChildren()** in *stdCallback* are 
of type **ObjCompCallType**. You can use one of the following values to 
specify how the buffer in *cbData* will be passed on to the next child's callback 
routine:

OCCT_SAVE_PARAMS_TEST_ABORT  
Save the buffer passed in *cbData* before calling each child; 
abort the routine if the callback returns true.

OCCT_SAVE_PARAMS_DONT_TEST_ABORT  
Save the buffer passed in *cbData* before calling each child; do 
not check the return value of the callback before proceeding to 
the next child.

OCCT_DONT_SAVE_PARAMS_TEST_ABORT  
Do not save the buffer in *cbData*, and abort if the callback 
routine returns true.

OCCT_DONT_SAVE_PARAMS_DONT_TEST_ABORT  
Do not save the buffer in *cbData*, and do not check the callback 
routine's return value.

OCCT_ABORT_AFTER_FIRST  
Abort the processing after only one child (typically used to call 
the *nth* child).

OCCT_COUNT_CHILDREN  
Counts the number of children rather than calling each.

**Include:** metaC.goh

**See Also:** @send, @call, MSG_META_SEND_CLASSED_EVENT

----------
#### ObjCompRemoveChild()
    void    ObjCompRemoveChild(
            optr    obj,            /* parent's optr */
            optr    objToRemove     /* optr of child to be removed */
            word    flags,          /* CompChildFlags */
            word    masterOffset,   /* offset to master part */
            word    compOffset,     /* offset to comp field in master part */
            word    linkOffset);    /* offset to link field in master part */

This routine removes the given child from the specified parent composite. 
The child will be removed entirely from the object tree, but it will not be 
detached or freed. The parameters of this routine are listed below:

*obj* - The optr of the parent composite.

*objToRemove* - The optr of the child to be removed.

*flags* - A record of **CompChildFlags** indicating whether the parent 
and child should be marked dirty after the operation.

*masterOffset* - The offset within the parent's instance chunk to the master 
group's offset. (The value that would appear in the parent class' 
*Class_masterOffset* field in its **ClassStruct** structure.)

*compOffset* - The offset within the parent's instance chunk to the composite 
field.

*linkOffset* - The offset within the parent's instance chunk to the link field.

**Include:** metaC.goh

----------
#### ObjDecInteractibleCount()
    void    ObjDecInteractibleCount(
            MemHandle mh);          /* subject object block */

This routine decrements the given object block's interactable count. Do not 
decrement the interactable count without first incrementing it with 
**ObjIncInteractibleCount()**. Visible objects automatically decrement the 
interactable count in their MSG_VIS_CLOSE handlers.

**Include:** object.h

**See Also:** ObjIncInteractibleCount(), MSG_VIS_CLOSE, ObjLMemBlockHeader

----------
#### ObjDecInUseCount()
    void    ObjDecInUseCount(
            MemHandle mh);      /* subject object block */

This routine decrements the given object block's in-use count. When the 
in-use count reaches zero, the block may safely be freed. You should not 
decrement the in-use count of a block without first incrementing it at some 
point with **ObjIncInUseCount()**.

**Warnings:** Do not decrement the in-use count without incrementing it first. An error will 
result.

**Include:** object.h

**See Also:** ObjIncInUseCount(), ObjDecInteractibleCount(), ObjLMemBlockHeader

----------
#### ObjDeref()
    void    * ObjDeref(
            optr    obj             /* optr to dereference */
            word    masterLevel);   /* specific master level to dereference */

This routine dereferences the given optr and master level to reset the 
message parameter *pself*. Because many routines and messages may cause 
the calling object's instance chunk to move, the *pself* parameter may become 
invalid. The two parameters to **ObjDeref()** are

*obj* - The optr of the object to be dereferenced; nearly always you will 
want to pass **oself**.

*masterLevel* - The master level of the part to be dereferenced. This is the 
offset into the instance chunk where the offset to the master 
part is stored. Since *pself* points to the first byte of a master 
part, you must specify which master part you are 
dereferencing.

For example, a visible object dereferencing its **VisClass** instance data would 
call this routine as follows:

    pself = ObjDeref(oself, 4);

Note, however, the **ObjDeref1()** and **ObjDerefVis()** exist to dereference the 
Vis master part, and **ObjDeref2()** and **ObjDerefGen()** exist to dereference 
the Gen master part.

**Include:** object.h

**See Also:** ObjDeref1(), ObjDeref2()

----------
#### ObjDerefHandles()
    void    * ObjDerefHandles(
            MemHandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            word            masterLevel);   /* master level to dereference */

This routine is exactly the same as **ObjDeref()**, above, except that the optr 
is specified as its separate handles.

**Include:** object.h

----------
#### ObjDeref1()
    void    * ObjDeref1(
            optr obj);          /* optr of object to be dereferenced */

This routine is a special version of **ObjDeref()** which dereferences the first 
master part of an object. Visible objects should use this routine or 
**ObjDerefVis()** instead of **ObjDeref()**.

**Include:** object.h

**See Also:** ObjDeref(), ObjDeref2()

----------
#### ObjDeref1Handles()
    void    *ObjDeref1Handles(
            MemHandle       mh,         /* handle portion of optr */
            ChunkHandle     ch,);       /* chunk handle portion of optr */

This routine is exactly like **ObjDeref1()**, above, except that the optr is 
specified as its separate handles.

**Include:** object.h

----------
#### ObjDeref2()
    void    * ObjDeref2(
            optr    obj);       /* optr of object to be dereferenced */

This routine is a specialized version of **ObjDeref()** which dereferences the 
second master part of an object. Generic objects should use this routine or 
**ObjDerefGen()** instead of **ObjDeref()**.

**Include:** object.h

**See Also:** ObjDeref(), ObjDeref1()

----------
#### ObjDeref2Handles()
    void    * ObjDeref2Handles(
            MemHandle       mh,/            /* handle portion of optr */
            ChunkHandle     ch);            /* chunk portion of optr */

This routine is exactly like **ObjDeref2()**, above, except that the optr is 
specified as its separate handles.

**Include:** object.h

----------
#### ObjDerefGen()
    void    * ObjDerefGen(
            optr    obj);           /* generic object to be dereferenced */

This routine is exactly the same as **ObjDeref2()** and dereferences the Gen 
master part of a generic object.

**Include:** object.h

**See Also:** ObjDeref(), ObjDeref2()

----------
#### ObjDerefVis()
    void    * ObjDerefVis(
            optr    obj);           /* visible object to be dereferenced */

This routine is exactly the same as **ObjDeref1()** and dereferences the Vis 
master part of a visible object or a visibly-realized generic object.

**Include:** object.h

**See Also:** ObjDeref(), ObjDeref1()

----------
#### ObjDoRelocation()
    Boolean ObjDoRelocation( /* returns true if error */
            ObjRelocationType   type,           /* type of relocation */
            MemHandle           block,          /* handle of info block */
            void                * sourceData,   /* source of relocation */
            void                * destData);    /* relocated value */

This routine relocates a given word or dword argument and is used for 
resolving handles and pointers to movable objects. Most often, relocation and 
unrelocation occur when resources are loaded, swapped, or saved, and this is 
in most cases taken care of by the kernel.

**ObjDoRelocation()** takes four parameters:

*type* - The type of relocation to be performed (**RelocationType**). This 
can be one of the three values shown below.

*block* - The handle of the block containing the relocation.

*sourceData* - A pointer to the source of the relocation; this pointer should be 
cast to the proper type (word or dword) when calling the 
routine.

*destData* - A pointer to the value to be returned; this pointer should be 
cast appropriately when the routine is called. The exact type of 
return value depends on *sourceData* and *type*, above.

The type of relocation to be done affects the type of data passed in *sourceData* 
and *destData*. The relocation type is passed in the type parameter and must 
be one of the following enumerations of **RelocationType**:

RELOC_RELOC_HANDLE  
The relocation will be from a resource ID to a handle. The 
*sourceData* pointer should be cast to type word, and the 
*destData* pointer should be cast to type Handle.

RELOC_RELOC_SEGMENT  
The relocation will be from a resource ID to a segment address. 
The *sourceData* pointer should be cast to type word, and the 
*destData* pointer should be cast to type Segment.

RELOC_RELOC_ENTRY_POINT  
The relocation will be from either a resource ID or an entry 
number to an entry point. Both the *sourceData* pointer and the 
*destData* pointer should be cast to type dword.

**ObjDoRelocation()** returns an error flag that will be *true* if an error occurs, 
*false* otherwise.

The relocation done by this routine can be undone with 
**ObjDoUnRelocation()**.

**Include:** object.h

----------
#### ObjDoUnRelocation()
    Boolean ObjDoUnRelocation( /* returns true if error */
            ObjRelocationType   type,           /* type of relocation */
            MemHandle           block,          /* handle of info block */
            void                * sourceData,   /* source of relocation */
            void                * destData);    /* relocated value */

This routine unrelocates a given word or dword. It translates a handle, a 
segment address, or an entry point back into a resource ID. The translation 
done is the exact reverse of that done by **ObjDoRelocation()**. See that 
routine (above) for more information.

**ObjDoUnRelocation()** returns an error flag that will be *true* if an error 
occurs and *false* if the unrelocation is successful. The unrelocated resource ID 
will be returned pointed to by the *destData* pointer.

**Include:** object.h

**See Also:** ObjDoRelocation()

----------
#### ObjDuplicateMessage()
    EventHandle ObjDuplicateMessage(
            EventHandle msg);               /* event to duplicate */

This routine duplicates a prerecorded event, returning the event handle of 
the new event. Pass the handle of the event to be duplicated. You can then 
change information about the event with **ObjSetMessageDestination()**.

**Include:** object.h

**See Also:** ObjSetEventInfo()

----------
#### ObjDuplicateResource()
    MemHandle ObjDuplicateResource(
            MemHandle       blockToDup,     /* handle of resource; must
                                             * not be loaded */
            GeodeHandle     owner,          /* owner of duplicate */
            ThreadHandle    burdenThread);  /* burden thread of duplicate */

This routine duplicates an entire object resource block. The new block will be 
put on the "saved blocks" list so it gets saved to the geode's state file. Usually 
this is used by the UI to make editable copies of an application's UI resources 
to ensure the proper state information gets saved. This routine takes three 
parameters:

*blockToDup* - The handle of the block to be duplicated. The block must not be 
resident in memory when **ObjDuplicateResource()** is called. 
Also, it can only be a "template" resource - a resource that does 
not get used by the UI or the application directly, but only gets 
copied via this routine.

*owner* - The owner geode of the new block. This should be the geode 
handle of the owning geode or zero to have the calling geode 
own it. If you pass an *owner* of -1, the new block will be owned 
by the same geode that owns the original.

*burdenThread* - The thread that will run the block and handle its messages. 
This should be a thread handle or zero to have the calling 
thread run the block. Passing a *burdenThread* of -1 makes the 
new resource have the same burden thread as the original.

**ObjDuplicateResource()** returns the handle of the newly created block, 
which will be unlocked and may or may not be resident in memory.

**Include:** object.h

**See Also:** ObjFreeDuplicate(), MSG_META_BLOCK_FREE, ObjLockObjBlock()

----------
#### ObjEnableDetach()
    void    ObjEnableDetach(
            optr    obj);       /* object calling this routine */

This routine acts as an object's handler for MSG_META_ACK. This handler 
decrements the acknowledgment count (incremented with **ObjIncDetach()**) 
and, if the count is zero, enables the detach mechanism so the object can be 
fully detached. Because the detach mechanism is implemented in 
**MetaClass**, it is highly unlikely you will ever call this routine.

The lone parameter of this routine is the optr of the calling object (or, in the 
case of MSG_META_ACK, the object sending acknowledgment).

**Warnings:** This routine may resize and/or move chunks and object blocks, thereby 
invalidating all pointers and segment addresses.

**Include:** metaC.goh

**See Also:** MSG_META_DETACH, ObjInitDetach(), ObjIncDetach(), MSG_META_ACK

----------
#### ObjFreeChunk()
    void    ObjFreeChunk(
            optr    o);     /* optr of chunk to be freed */

This routine frees the passed object's instance chunk. If the object came from 
a loaded resource, however, the object is resized to zero and marked dirty 
rather than actually freed.

**Warnings:** The object must be fully detached, and its message queues must be empty 
before it can safely be freed. All this is handled by MSG_META_DETACH and 
MSG_META_OBJ_FREE.

**Include:** object.h

**See Also:** MSG_META_DETACH, MSG_META_OBJ_FREE, ObjInstantiate()

----------
#### ObjFreeChunkHandles()
    void    ObjFreeChunkHandles(
            MemHandle       mh,         /* handle portion of optr */
            ChunkHandle     ch);        /* chunk portion of optr */

This routine is the same as **ObjFreeChunk()**; the chunk is specified by its 
handles rather than by an optr.

**Include:** object.h

----------
#### ObjFreeDuplicate()
    void    ObjFreeDuplicate(
            MemHandle mh);      /* handle of duplicate block to be freed */

This routine frees a block that had been saved with **ObjSaveBlock()** or 
created with **ObjDuplicateResource()**. It must be passed the memory 
handle of the duplicated resource.

**Warnings:** All objects in the duplicated resource must be properly detached to ensure 
that nothing tries to send messages to the objects in the block. Additionally, 
the block's in-use count and interactable count should be zero.

**Include:** object.h

**See Also:** ObjDuplicateResource(), ObjSaveBlock(), ObjLMemBlockHeader

----------
#### ObjFreeMessage()
    void    ObjFreeMessage(
            EventHandle event);         /* event to be freed */

This routine frees an event handle and its associated event. This is rarely, if 
ever, used by anything other than the kernel. The kernel uses this routine to 
free events after they have been handled.

**Include:** object.h

----------
#### ObjFreeObjBlock()
    void    ObjFreeObjBlock(
            MemHandle block);   /* handle of the object block to be freed */

This routine frees the specified object block. It first checks the block's in-use 
count to see if any external references to the block are being kept. If the 
in-use count is nonzero, **ObjFreeObjBlock()** simply sets the block's 
auto-free bit and returns; the block will be freed the first time the in-use 
count reaches zero. If the in-use count is zero (no external references), the 
block will be freed immediately.

If the object block passed is not run by the calling thread, the operation will 
be handled by a remote call in the object block's thread.

**Include:** object.h

**See Also:** ObjFreeDuplicate(), MSG_META_BLOCK_FREE

----------
#### ObjGetFlags()
    ObjChunkFlags ObjGetFlags(
            optr    o);     /* optr of subject object */

This routine returns the object flags associated with a given object. The object 
is specified by the passed optr, and the flags are stored in the object's 
**ObjChunkFlags** record.

**Include:** object.h

**See Also:** ObjSetFlags(), ObjChunkFlags

----------
#### ObjGetFlagsHandles()
    ObjChunkFlags ObjGetFlagsHandles(
            Memhandle       mh,         /* handle portion of optr */
            ChunkHandle     ch);        /* chunk portion of optr */

This routine is the same as **ObjGetFlags()**, but the object is specified with 
its handles rather than with its optr.

**Include:** object.h

----------
#### ObjGetMessageInfo()
    Message ObjGetMessageInfo(
            EventHandle     event,      /* event to be queried */
            optr            * dest);    /* buffer for destination optr */

This routine gets information about the specified *event*. The return value is 
the message number of the event. The *dest* parameter is a pointer to an optr. 
This routine will return with the optr represinting the event's destination 
object.

**Include:** object.h

----------
#### ObjIncDetach()
    void    ObjIncDetach(
            optr    obj);       /* optr of calling object */

This routine increments the number of detach acknowledgments an object 
must receive before it can safely be detached. Each time the detaching object 
sends notification of its detachment, it must call **ObjIncDetach()** to indicate 
that it must receive a corresponding detach acknowledgment 
(MSG_META_ACK).

The calling object must have previously called **ObjInitDetach()**. Since the 
detach mechanism is implemented in **MetaClass**, it is highly unlikely you 
will ever need to call this routine. **ObjIncDetach()** takes a single parameter: 
the optr of the calling object.

**Include:** metaC.goh

**See Also:** MSG_META_DETACH, ObjInitDetach(), ObjEnableDetach(), MSG_META_ACK

----------
#### ObjIncInteractibleCount()
    void    ObjIncInteractibleCount(
            MemHandle mh);          /* handle of object block */

This routine increments the interactable count of the given object block. The 
interactable count maintains the number of objects currently visible to the 
user or about to be acted on by the user (e.g. via keyboard accelerator). The 
interactable count is maintained by the UI; only in extremely special cases 
may you need to increment or decrement the count. To decrement the count, 
use **ObjDecInteractibleCount()**.

Visible objects increment the interactable count in their MSG_VIS_OPEN 
handlers and decrement it in their MSG_VIS_CLOSE handlers. This is built 
into **VisClass**.

**Include:** object.h

**See Also:** ObjDecInteractibleCount(), MSG_VIS_OPEN, MSG_VIS_CLOSE, 
ObjLMemBlockHeader

----------
#### ObjIncInUseCount()
    void    ObjIncInUseCount(
            MemHandle mh);          /* handle of object block */

This routine increments the given object block's in-use count. The in-use 
count maintains the number of outside references to this object block which 
are stored elsewhere and which need to be removed before the block can 
safely be freed. If you store an optr to an object block, you should increment 
the in-use count of the block.

When the reference to the block is removed, the in-use count should be 
decremented with **ObjDecInUseCount()**.

**Include:** object.h

**See Also:** ObjDecInUseCount(), ObjIncInteractibleCount(), ObjLMemBlockHeader

----------
#### ObjInitDetach()
    void    ObjInitDetach(
            MetaMessages    msg,
            optr            obj         /* object being detached */
            word            callerID,   /* an identifier token for the caller */
            optr            ackOD);     /* object to which ack is ent */

Initialize the detach sequence for the specified object. The detach sequence 
severs all ties between the system and the object, allowing it to be destroyed 
without other objects or geodes trying to contact it. It is highly unlikely you 
will ever call this routine; typically, you will instead use MSG_META_DETACH 
or one of the generic or visible object messages, which will call this routine. 
The parameters for this routine are

*msg* - The detach message.         

*obj* - The optr of the object to be detached.

*callerID* - The caller object's ID.

*ackOD* - The optr of the caller object or another object which is to receive 
acknowledgment notification of the detach.

**Include:** metaC.goh

**See Also:** MSG_META_DETACH, MSG_GEN_DESTROY, MSG_VIS_REMOVE, 
ObjIncDetach(), ObjEnableDetach(), MSG_META_ACK

----------
#### ObjInitializeMaster()
    void    ObjInitializeMaster(
            optr            obj,        /* object to be initialized */
            ClassStruct     * class);   /* class in master group */

This routine initializes the appropriate master part of the passed object, 
resizing the instance chunk if necessary. It takes two parameters:

*obj* - The optr of the object whose master part is to be initialized.

*class* - A pointer to the class definition of a class in the appropriate 
master group. This does not have to be the master class; it must 
only be a class in the master group.

**Warnings:** This routine may resize and/or move chunks or object blocks, thereby 
invalidating pointers and segment addresses.

**Include:** object.h

**See Also:** ObjResizeMaster(), ObjInitializePart(), ClassStruct

----------
#### ObjInitializeMasterHandles()
    void    ObjInitializeMasterHandles(
            MemHandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            ClassStruct     * class);       /* class in master group */

This routine is the same as **ObjInitializeMaster()** except it specifies the 
object via its handles rather than its optr.

**Include:** object.h

----------
#### ObjInitializePart()
    void    ObjInitializePart(
            optr    obj,            /* object to have a part initialized */
            word    masterOffset);  /* offset to master offset in chunk */

This routine initializes all master parts of the given object down to and 
including the master part specified in *masterOffset*. It will resize the chunk 
if necessary and even resolve variant classes above the master group 
specified, if necessary. This routine takes two parameters:

*obj* - The optr of the object to be initialized.

*masterOffset* - The offset within the parent's instance chunk to the master 
group's offset (the value that would appear in the parent class' 
*Class_masterOffset* field in its **ClassStruct** structure).

**Warnings:** This routine may move and/or resize chunks or object blocks, thereby 
invalidating pointers and segment addresses.

**Include:** object.h

**See Also:** ObjResizeMaster(), ObjInitializeMaster(), 
MSG_META_RESOLVE_VARIANT_SUPERCLASS

----------
#### ObjInitializePartHandles()
    void    ObjInitializePartHandles(
            Memhandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            word            masterOffset);  /* master group offset */

This routine is the same as **ObjInitializePart()** except that it specifies the 
object via its handles rather than an optr.

**Include:** object.h

----------
#### ObjInstantiate()
    optr    ObjInstantiate(
            MemHandle       block,      /* block in which new object
                                         * will be instantiated */
            ClassStruct     * class);   /* class of new object */

This routine instantiates a new object, allocating the proper size instance 
chunk. It returns the optr of the new object; this optr can then be used to send 
setup messages or other messages (such as adding the object to an object tree, 
setting it usable, etc.).

The new object's instance data will be initialized to all zeroes if it has no 
master parts (is a direct descendant of **MetaClass**). If it is a member of some 
master group, only enough space for the base structure (the master offsets 
and the class pointer) will be allocated. In either case, initialization of the 
instance data will occur at a later time.

**ObjInstantiate()** takes two parameters:

*block* - The memory handle of an object block in which the object's 
instance chunk will be allocated. This block *must* be an object 
block, though it need not be run by the caller's thread. If the 
block is run by another thread, the routine will be executed as 
a remote call.

*class* - A pointer to the **ClassStruct** structure of the class of the new 
object. This pointer will be set in the object's class pointer (the 
first four bytes of the instance chunk).

**Warnings:** This routine, because it allocates a new chunk, may cause LMem and Object 
blocks to move or resize, thereby invalidating any pointers and segment 
addresses. Be sure to dereference pointers after calls to this routine.

**Include:** metaC.goh

----------
#### ObjIsClassADescendant()
    Boolean ObjIsClassADescendant(
            ClassStruct     * class1,           /* proposed ancestor */
            ClassStruct     * class2);          /* proposed descendant */

This routine checks if *class2* is a descendant of *class1* and returns *true* if it is.

**Include:** object.h

----------
#### ObjIsObjectInClass()
    Boolean ObjIsObjectInClass(
            optr            obj,            /* object to check */
            ClassStruct     * class);       /* proposed class */

This routine checks to see if the passed object is a member of the specified 
class. It checks superclasses as well, but if an unresolved variant class is 
encountered, the variant will *not* be resolved. If you want to search past 
variant class links, you should sent MSG_META_DUMMY to the object first. 
The two parameters for this routine are

*obj* - The optr of the object to be checked.

*class* - A pointer to the subject class' **ClassStruct** definition.

**ObjIsObjectInClass()** returns *true* if the object is in the class, *false* (zero) if 
it is not.

**Include:** object.h

----------
#### ObjIsObjectInClassHandles()
    Boolean ObjIsObjectInClassHandles(
            MemHandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            ClassStruct     * class);       /* proposed class */

This routine is just like **ObjIsObjectInClass()** except the object is specified 
via its handles rather than its optr.

**Include:** object.h

----------
#### ObjLinkFindParent()
    optr    ObjLinkFindParent(
            optr    obj,            /* child's optr */
            word    masterOffset,   /* offset to master part with link field */
            word    linkOffset);    /* offset in master part to link field */

This routine returns the optr of the specified object's parent. It must be 
passed the following:

*obj* The optr of the object whose parent is to be found.

*masterOffset* - The offset within the object's instance chunk to its master 
group's offset (the value that would appear in the 
*Class_masterOffset* field in its **ClassStruct** structure).

*linkOffset* - The offset within the object's instance chunk to the link field.

**Include:** metaC.goh

**See Also:** MSG_VIS_FIND_PARENT, MSG_GEN_FIND_PARENT

----------
#### ObjLockObjBlock()
    void    * ObjLockObjBlock(
            MemHandle mh);          /* handle of object block */

This routine locks an object block, loading in the block if necessary. It must 
be passed the handle of the block, and it returns the locked block's segment 
address. When the caller is done using the block, it should unlock it with 
**MemUnlock()**.

**Be Sure To:** Always unlock the block when you are done with it, with **MemUnlock()**.

**Include:** object.h

**See Also:** MemLock(), MemUnlock()

----------
#### ObjMapSavedToState()
    VMBlockHandle ObjMapSavedToState(
        MemHandle mh);          /* handle of object block */

This routine returns the VM block handle of the state file block corresponding 
to the passed object block. If the specified object block has no state file 
equivalent, a null handle is returned.

**Include:** object.h

----------
#### ObjMapStateToSaved()
    MemHandle ObjMapStateToSaved(
            VMBlockHandle   vmbh,   /* VM block handle of state block */
            GeodeHandle     gh);    /* handle of geode owning block */

This routine takes a VM block handle and a geode handle and returns the 
memory block corresponding to the VM block, if any. The two parameters are

*vmbh* - The VM block handle of the VM block to be mapped.

*gh* - The geode handle of the owner of the block, or zero to use the 
calling geode's handle.

If the block is found, **ObjMapStateToSaved()** returns its handle. If the 
block is not found, it returns a null handle.

**Include:** object.h

----------
#### ObjMarkDirty()
    void    ObjMarkDirty(
            optr    o);         /* object to be marked dirty */

This routine marks an object dirty, indicating that changes to it should be 
saved when its object block is saved. If you want changes to objects saved, you 
should mark the object dirty.

**Tips and Tricks:** Often you do not need this routine because parameters or flags to other 
routines will set the object dirty automatically. If there is any doubt, however, 
you should use this routine.

**Include:** object.h

**See Also:** ObjChunkFlags, ObjSetFlags()

----------
#### ObjMarkDirtyHandles()
    void    ObjMarkDirtyHandles(
            MemHandle       mh,         /* handle portion of optr */
            ChunkHandle     ch);        /* chunk portion of optr */

This routine is the same as **ObjMarkDirty()** except that it specifies the 
object via handles rather than an optr.

----------
#### ObjProcBroadcastMessage()
    void    ObjProcBroadcastMessage(
            EventHandle event);         /* the event to be broadcast */

This routine broadcasts the given event to all threads which have message 
queues. It must be passed as an encapsulated event (usually recorded with 
**@record**) and returns nothing. This is typically used for notification 
purposes.

**Include:** metaC.goh

----------
#### ObjRelocateEntryPoint()
    void *  ObjRelocateEntryPoint(
            EntryPointRelocation *          relocData);

----------
#### ObjRelocOrUnRelocSuper()
    void    ObjRelocOrUnRelocSuper(
            optr            oself,
            ClassStruct     *class,
            word            frame);

Call this routine to relocate an object's superclass.

**Include:** object.h

----------
#### ObjResizeMaster()
    void    ObjResizeMaster(
            optr    obj,            /* object to have its master part resized */
            word    masterOffset,   /* master offset of proper master part */
            word    newSize);       /* new size for the master part */

This routine resizes a master part of an object's instance chunk. It is typically 
used to allocate space for a master part or to resize the master part to zero 
(as when the Vis part of a visible object is removed in MSG_VIS_CLOSE). This 
routine must be passed the following three parameters:

*obj* - The optr of the object whose master part is to be resized.

*masterOffset* - The offset into the object's instance chunk where the offset to 
the master part is kept (this is the same offset held in the 
master class' *Class_masterOffset* field).

*newSize* - The new size of the master part. This can be found in the 
master class' *Class_instanceSize* field.

**Warnings:** This routine may resize and/or move chunks or object blocks, thereby 
invalidating stored segment addresses and pointers.

**Include:** object.h

**See Also:** ClassStruct, ObjInitializeMaster(), ObjInitializePart()

----------
#### ObjResizeMasterHandles()
    void    ObjResizeMasterHandles(
            MemHandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            word            masterOffset,   /* offset to master part */
            word            newSize);       /* new size of master part */

This routine is the same as **ObjResizeMaster()** except that the object is 
specified with its handles rather than its optr.

**Include:** object.h

----------
#### ObjSaveBlock()
    void    ObjSaveBlock(
            MemHandle   mh);    /*handle of block to be marked for saving */

This routine sets up an object or LMem block to be saved to its owner's state 
file. The block's handle must be passed in *mh*. The block must be an object 
block.

**Include:** object.h

**See Also:** ObjMapSavedToState(), ObjMapStateToSaved()

----------
#### ObjSetEventInfo()
    void    ObjSetEventInfo(
            EventHandle     event,      /* handle of the event to be modified */
            Message         msg,        /* the new message for the event */
            optr            dest);      /* the new destination of the event */

This routine modifies an event, setting its information to the passed values. 
The three parameters are

*event* - The event handle of the event to be modified.

*msg* - The message to be sent in place of the current message. To use 
the same message, you must first retrieve it with 
**ObjGetMessageInfo()**.

*dest* - The optr of the new destination object for the event. To use the 
same destination object, you must first retrieve it with 
**ObjGetMessageInfo()**.

**Include:** object.h

**See Also:** ObjGetEventInfo()

----------
#### ObjSetFlags()
    void    ObjSetFlags(
            optr            o,              /* object whose flags will be set */
            ObjChunkFlags   bitsToSet,      /* flags to set */
            ObjChunkFlags   bitsToClear);   /* flags to clear */

This routine sets the chunk flags for the specified object. Flags that should be 
set are passed in *bitsToSet*, and flags that should be cleared are passed in 
*bitsToClear*. Typically, applications will not use this routine but will rather 
let the kernel maintain the object's flags.

**Include:** object.h

**See Also:** ObjGetFlags(), ObjChunkFlags

----------
#### ObjSetFlagsHandles()
    void    ObjSetFlagsHandles(
            MemHandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            ObjChunkFlags   bitsToSet,      /* flags to set */
            ObjChunkFlags   bitsToClear);   /* flags to clear */

This routine is the same as **ObjSetFlags()** except that the object is specified 
via its handles rather than its optr.

**Include:** object.h

----------
#### ObjTestIfObjBlockRunByCurThread()
    Boolean ObjTestIfObjBlockRunByCurThread(
            MemHandle   mh);        /* handle of object block */

This routine checks if the calling thread is running the specified object block. 
This routine can be used to determine if calls to objects in the block are across 
threads or internal to the calling thread. Pass this routine the handle of the 
object block to be checked - if the object block is a VM block, the thread for the 
VM file is checked rather than that for the block.

If the block is run by the calling thread, the return value is *true*. If a different 
thread runs the block, the return is *false* (zero).

**Include:** object.h

----------
#### ObjUnrelocateEntryPoint()
    void    ObjUnrelocateEntryPoint(
            EntryPointRelocation *      relocData,
            void *                      entryPoint);

----------
#### ObjVarAddData()
    void    * ObjVarAddData(
            optr        obj,            /* object to add vardata to */
            VardataKey  dataType,       /* vardata type */
            word        dataSize);      /* vardata data size, if any */

This routine adds or alters a variable data entry for the specified object. If the 
data type does not currently exist in the instance chunk, it will be allocated 
and added to the chunk. If it does exist, the extra data of the entry will be 
re-initialized to all zeroes.

This routine returns a pointer to the extra data of the new or modified entry; 
if the entry has no extra data, an opaque pointer to the entry is passed, and 
you can use this pointer with **ObjVarDeleteDataAt()**. In either case, the 
object will be marked dirty.

The parameters of this routine are

*obj* - The optr of the object affected. This should be the caller's optr.

*dataType* - The **VardataKey** word declaring the data type and its flags. 
The VDF_SAVE_TO_STATE flag must be properly set; the 
VDF_EXTRA_DATA flag is ignored, however, as the routine will 
set it properly.

*dataSize* - The size of the extra data for this type. If the type has no extra 
data, pass zero.

**Include:** object.h

**Warnings:** This routine should be called only by the object whose vardata is affected. To 
operate on other objects' vardata remotely, use messages provided by 
**MetaClass** (see below under "See Also").

**See Also:** MSG_META_ADD_VAR_DATA, ObjVarDeleteDataAt()

----------
#### ObjVarAddDataHandles()
    void    * ObjVarAddDataHandles(
            MemHandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            VardataKey      dataType,       /* vardata type */
            word            dataSize);      /* vardata data size, if any */

This routine is the same as **ObjVarAddData()** except that the object is 
specified via its handles rather than its optr.

**Include:** object.h

----------
#### ObjVarCopyDataRange()
    void    ObjVarCopyDataRange(
            optr    source,     /* the optr of the source object */
            optr    dest,       /* the optr of the destination (calling) object */
            word    rangeStart, /* the smallest data type value to be copied */
            word    rangeEnd);  /* the largest data type value to be copied */

This routine copies all the vardata entries within the specified range from the 
*source* object to the *dest* object. The range to be copied is specified by data 
types and is between *rangeStart* and *rangeEnd*, inclusive. If any data entries 
are copied, the destination object will be marked dirty.

**Warnings:** This routine should be called only by the destination object; it is against OOP 
doctrine for one object to alter another's instance data.

**Include:** object.h

----------
#### ObjVarDeleteData()
    Boolean ObjVarDeleteData(
            optr        obj,            /* object to delete from */
            VardataKey  dataType);      /* data type to delete */

This routine deletes a vardata entry from the specified object's instance 
chunk, if the entry exists. The entry is specified by its data type; to delete an 
entry specified by a pointer to it, use **ObjVarDeleteDataAt()**, below. It 
returns an error flag: *true* if the entry was not found, *false* if the entry was 
successfully deleted. The object will also be marked dirty by the routine.

The parameters for this routine are

*obj* - The optr of the object affected. This should be the caller's optr.

*dataType* - The **VardataKey** word declaring the data type and its flags. 
Both the VDF_SAVE_TO_STATE flag and the VDF_EXTRA_DATA 
flag are ignored.

**Warnings:** This routine should be called only by the object whose vardata is affected. To 
operate on other objects' vardata remotely, use messages provided by 
**MetaClass** (see below under "See Also").

**Include:** object.h

**See Also:** MSG_META_DELETE_VAR_DATA, ObjVarDeleteDataAt()

----------
#### ObjVarDeleteDataHandles()
    Boolean ObjVarDeleteDataHandles(
            MemHandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            VardataKey      dataType);      /* data type to delete */

This routine is the same as **ObjVarDeleteData()** except that the object is 
specified via its handles rather than its optr.

**Include:** object.h

----------
#### ObjVarDeleteDataAt()
    void    ObjVarDeleteDataAt(
            optr    obj,                /* object to delete from */
            word    extraDataOffset);   /* offset to extra data to delete */

This routine deletes the specified vardata entry from the given object's 
instance chunk. The vardata entry is specified by its pointer as returned by 
**ObjVarAddData()**, **ObjVarFindData()**, and **ObjVarDerefData()**. To 
delete an entry specified by its data type, use **ObjVarDeleteData()**, above.

**Warnings:** This routine should be called only by the object whose vardata is affected. To 
operate on other objects' vardata remotely, use messages provided by 
**MetaClass** (see below under "See Also").

**Include:** object.h

**See Also:** MSG_META_DELETE_VAR_DATA, ObjVarDeleteData()

----------
#### ObjVarDeleteDataAtHandles()
    void    ObjVarDeleteDataAtHandles(
            MemHandle       mh,         /* handle portion of optr */
            ChunkHandle     ch,         /* chunk portion of optr */
            word    extraDataOffset);   /* offset to extra data to delete */

This routine is the same as **ObjVarDeleteDataAt()** except that the object is 
specified via its handles rather than its optr.

**Include:** object.h

----------
#### ObjVarDeleteDataRange()
    void    ObjVarDeleteDataRange(
            optr        obj,                /* object to delete from */
            word        rangeStart,         /* start of range */
            word        rangeEnd,           /* end of range */
            Boolean     useStateFlag);      /* save to state flag */

This routine deletes all data entries within a given range for the passed 
object. The range is specified by beginning and ending data types and is 
inclusive. The four parameters to this routine are

*obj* - The optr of the object whose data entries are to be deleted.

*rangeStart* - The lowest number data type to be deleted. Both the 
VDF_SAVE_TO_STATE flag and the VDF_EXTRA_DATA flag are 
ignored.

*rangeEnd* - The highest number data type to be deleted. Both the 
VDF_SAVE_TO_STATE flag and the VDF_EXTRA_DATA flag are 
ignored.

*useStateFlag* - A flag indicating whether entries with their 
VDF_SAVE_TO_STATE flags should be deleted. Pass *true* 
(nonzero) to take the state flag into account; pass *false* (zero) to 
delete all entries in the range.

**Warnings:** This routine should be called only by the object whose vardata is affected. To 
operate on other objects' vardata remotely, use messages provided by 
**MetaClass** (see below under "See Also").

**Include:** object.h

**See Also:** MSG_META_DELETE_VAR_DATA

----------
#### ObjVarDeleteDataRangeHandles()
    void    ObjVarDeleteDataRangeHandles(
            MemHandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            word            rangeStart,     /* start of range */
            word            rangeEnd,       /* end of range */
            Boolean         useStateFlag);  /* save to state flag */

This routine is the same as **ObjVarDeleteDataRange()** except that the 
object is specified via its handles rather than its optr.

**Include:** object.h

----------
#### ObjVarDerefData()
    void    * ObjVarDerefData(
            optr            obj,            /* object having data type */
            VardataKey      dataType);      /* data type to dereference */

This routine is exactly like **ObjVarFindData()**, below, except that it does not 
return a null pointer if the data type is not found. Do not use this routine 
unless you are absolutely sure the data type is in the object; otherwise, 
results are unpredictable.

**Include:** object.h 

**See Also:** ObjVarFindData()

----------
#### ObjVarDerefDataHandles()
    void    * ObjVarDerefDataHandles(
            MemHandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            VardataKey      dataType);      /* data type to dereference */

This routine is the same as **ObjVarDerefData()** except that the object is 
specified via its handles rather than its optr.

**Include:** object.h 

----------
#### ObjVarFindData()
    void    * ObjVarFindData(
            optr            obj,            /* object to be checked */
            VardataKey      dataType);      /* data type to find */

This routine searches an object's variable data for a given data type. If the 
type is found, **ObjVarFindData()** returns a pointer to the entry's extra data; 
if the entry has no extra data, an opaque pointer is returned which may be 
used with **ObjVarDeleteDataAt()**. If the entry is not found, a null pointer 
is returned. The pointer returned by this routine must be used before any 
subsequent operations on the object's block; the pointer may be invalidated 
by other LMem or object operations.

The two parameters of this routine are

*obj* - The optr of the object affected. This should be the caller's optr.

*dataType* - The **VardataKey** word declaring the data type and its flags. 
Both the VDF_SAVE_TO_STATE flag and the VDF_EXTRA_DATA 
flag are ignored.

**Warnings:** This routine should be called only by the object whose vardata is affected. To 
operate on other objects' vardata remotely, use messages provided by 
**MetaClass** (see below under "See Also").

**Include:** object.h 

**See Also:** MSG_META_FIND_VAR_DATA

----------
#### ObjVarFindDataHandles()
    void    * ObjVarFindDataHandles(
            MemHandle       mh,             /* handle portion of optr */
            ChunkHandle     ch,             /* chunk portion of optr */
            VardataKey      dataType);      /* data type to find */

This routine is the same as **ObjVarFindData()** except that the object is 
specified via its handles rather than its optr.

**Include:** object.h 

----------
#### ObjVarScanData()
    void    ObjVarScanData(
            optr                obj,            /* object to be scanned */
            word                numHandlers,    /* number of handlers in table */
            VarDataCHandler     * handlerTable, /* pointer to handler table */
            void                * handlerData); /* pointer to handler data */

This routine scans an object's vardata and calls all the vardata handlers 
specified in the passed handler table. Pass it the following parameters:

*obj* - The optr of the object whose variable data table is to be 
scanned.

*numHandlers* - The number of handlers specified in the passed handler table.

*handlerTable* A pointer to a list of **VarDataCHandler** structures. Each of 
these structures contains a vardata data type and a pointer to 
the routine that is to handle it. All the handler routines must 
be in the same segment as the handler table.

*handlerData* - A pointer to a buffer that is passed on to the handlers. This can 
contain any information of specific interest to the application or 
handlers.

**Vardata Handler Format:** A vardata handler routine must have the following format:

    void _pascal (MemHandle mh, ChunkHandle chnk,
        VarDataEntry *extraData, word dataType,
        void *handlerData)

The handler should not free the object chunk or destroy the object; it can do 
anything else it pleases. The handler returns nothing and takes the following 
parameters:

*mh:chnk* - The memory handle and chunk handle of the object being 
referenced. Together, these comprise the optr of the object.

*extraData* - A pointer to the data type's extra data, if it has any. This 
pointer may be parsed with the macros **VarDataTypePtr()**, 
**VarDataFlagsPtr()**, and **VarDataSizePtr()**.

*dataType* - The data type of the data entry being handled. This is a record 
of type **VardataKey**.

*handlerData* - A pointer to a buffer passed through by **ObjVarScanData()**. 
This buffer may be used for passing additional data to the 
handlers.

**Structures:** The **VarDataCHandler** structure contains two elements:

    typedef struct {
        word            VDCH_dataType;
        void_pascal     (*VDCH_handler) (
                MemHandle       mh,
                ChunkHandle     chnk,
                VarDataEntry    * extraData,
                word            dataType
                void            * handlerData);
    } VarDataCHandler;

The first element is the data type, a record containing the data type and the 
vardata flags. The second element is a far pointer to the handler routine for 
the type.

**Include:** object.h 

----------
#### ObjVarScanDataHandles()
    void    ObjVarScanDataHandles(
            MemHandle           mh,             /* handle portion of optr */
            ChunkHandle         ch,             /* chunk portion of optr */
            word                numHandlers,    /* number of handlers in table */
            VarDataCHandler     * handlerTable, /* pointer to handler table */
            void                * handlerData); /* pointer to handler data */

This routine is the same as **ObjVarScanData()** except that the object is 
specified via its handles rather than its optr.

**Include:** object.h 

----------
#### offsetof()
    word    offsetof(struc, field);

This macro returns the offset of the specified field within the specified 
structure.

----------
#### OptrToChunk()
    ChunkHandle OptrToChunk(op);
            optr    op;

This macro extracts the chunk handle portion of the given optr.

**See Also:** ConstructOptr(), OptrToHandle()

----------
#### OptrToHandle()
    MemHandle OptrToHandle(op);
            optr    op;

This macro extracts the MemHandle portion of the given optr.

**See Also:** ConstructOptr(), OptrToChunk()

----------
#### ParallelClose()
    StreamError ParallelClose(
            GeodeHandle         driver,
            ParallelUnit        unit,
            Boolean             linger);

Close the stream to a parallel port.

**Include:** streamC.h 

----------
#### ParallelOpen()
    StreamError ParallelOpen(
            GeodeHandle         driver,
            ParallelUnit        unit,
            StreamOpenFlags     flags,
            word                outBuffSize,
            word                timeout);

This routine opens a stream to the specified parallel port. It is passed the 
following arguments:

*driver* - The **GeodeToken** of the parallel driver.

*unit* - The parallel port to open.

*flags* - This specifies whether the call should fail if the port is busy, or 
wait for a time to see if it will become free.

*outBuffSize* - The size of the stream buffer used for output to the parallel 
port.

*timeout* - The number of clock ticks to wait for the port to become free. 
(This argument is ignored if *flags* is not 
STREAM_OPEN_TIMEOUT.)

If the routine is successful, it returns zero. If it is unsuccessful, it returns a 
member of the **StreamError** enumerated type.

**Include:** streamC.h 

----------
#### ParallelWrite()
    StreamError ParallelWrite(
            GeodeHandle         driver,
            ParallelUnit        unit,
            StreamBlocker       blocker,
            word                buffSize,
            const byte *        buffer,
            word *              numBytesWritten);

Write data to a parallel port.

**Include:** streamC.h 

----------
#### ParallelWriteByte()
    StreamError ParallelWrite(
            GeodeHandle         driver,
            ParallelUnit        unit,
            StreamBlocker       blocker,
            word                buffSize,
            byte                dataByte);

Write one byte of data to a parallel port.

**Include:** streamC.h 

----------
#### PCB()
    #define PCB(return_type, pointer_name, args) \
            return_type _pascal (*pointer_name) args

This macro is useful for declaring pointers to functions that use the pascal 
calling conventions. For example, to declare a pointer to a function which is 
passed two strings and returns an integer, one could write

    PCB(int, func_ptr, (const char *, const char *));

which would be expanded to

    int _pascal (*func_ptr) (const char *, const char *);

**See Also:** CCB()

----------
#### PCCOMABORT()
    void PCCOMABORT(void);

This routine aborts the current file transfer operation being carried out by 
the PCCom library. It is the third entry point in the PCCom library.

**Include:** pccom.goh 

----------
#### PCCOMEXIT()
    PCComReturnType PCCOMEXIT();

This routine kills a pccom thread such as those started by PCCOMINIT(). It is 
the second entry point in the PCCom library.

**Structures:**

    typedef ByteEnum PCComReturnType;
    #define PCCRT_NO_ERROR 0
    #define PCCRT_CANNOT_LOAD_SERIAL_DRIVER 1
    #define PCCRT_CANNOT_CREATE_THREAD 2
    #define PCCRT_CANNOT_ALLOC_STREAM 3
    #define PCCRT_ALREADY_INITIALIZED 4

**Include:** pccom.goh 

----------
#### PCCOMINIT()
    PCComReturnType PCCOMINIT(
            SerialPortNum       port,
            SerialBaud          baud,
            word                timeout,
            optr                callbackOptr,
            PCComInitFlags      flags);

This entry point of the PCCom library spawns a new thread which monitors 
a serial port and acts as a passive pccom terminal. This routine is the first 
entry point in the PCCom library.

This routine takes the following arguments:

*port* - A **SerialPortNum** value specifying which serial port to use for 
the pccom connection. Pass -1 for the system default value: 
**com1** for the Zoomer, **com2** for the desktop product.

*baud* - A **SerialBaud** value specifying what speed to use. Pass -1 for 
the system default value: 19200 baud for the Zoomer, 38400 
baud for the desktop product.

*timeout* - Number of clock ticks (one tick is 1/60 second) to allow for 
connection.

*callbackOptr* - An object which will receive notification messages of certain 
events. A value of zero means no notification will be sent.

*flags* - If an object will be receiving notification messages, these flags 
determine what sort of notifications will be sent.

**Structures:**

    typedef ByteEnum PCComReturnType;
    #define PCCRT_NO_ERROR 0
    #define PCCRT_CANNOT_LOAD_SERIAL_DRIVER 1
    #define PCCRT_CANNOT_CREATE_THREAD 2
    #define PCCRT_CANNOT_ALLOC_STREAM 3
    #define PCCRT_ALREADY_INITIALIZED 4

    typedef WordFlags PCComInitFlags;
        /* send notifications when text is available for display */
    #define PCCIF_NOTIFY_OUTPUT 0x8000
        /* send notification when the remote machine shuts down the
         * serial line */
    #define PCCIF_NOTIFY_EXIT 0x4000

**Include:** pccom.goh 

----------
#### ProcCallFixedOrMovable_cdecl()
    dword   ProcCallFixedOrMovable_cdecl(
            void    (*routine),
            ...)

This routine calls the routine pointed to, passing the other arguments 
through to the called routine. The called routine must use C calling 
conventions.

**Include:** resource.h 

----------
#### ProcCallFixedOrMovable_pascal()
    dword   ProcCallFixedOrMovable_pascal(
            ...,
            void    (*routine))

This routine calls the routine pointed to, passing the other arguments 
through to the called routine. The called routine must use Pascal calling 
conventions.

**Include:** resource.h 

----------
#### ProcGetLibraryEntry()
    void *  ProcGetLibraryEntry(
            GeodeHandle     library,
            word            entryNumber)

This routine returns the pointer to a library's entry-point.

**Include:** resource.h 

----------
#### ProcInfo()
    ThreadHandle ProcInfo(
            GeodeHandle gh);        /* handle of geode to check */

This routine returns the first thread of the process geode specified. If the 
geode is not a process, the routine will return a null handle.

**Include:** geode.h 

----------
#### PtrToOffset()
    word    PtrToOffset(ptr);
            dword   ptr;

This macro returns just the lower 16 bits of the given dword. It is most useful 
for extracting the offset portion of a far pointer.

----------
#### PtrToSegment()
    word    PtrToSegment(ptr);
            dword   ptr;

This macro returns just the upper 16 bits of the given dword. It is most useful 
for extracting the segment address of a far pointer.

[Routines H-L](rrouth_l.md) <-- [Table of Contents](../routines.md) &nbsp;&nbsp; --> [Routines Q-T](rroutq_t.md)