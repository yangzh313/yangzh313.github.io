- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [web.stanford.edu](https://web.stanford.edu/class/cs346/2015/redbase-pf.html#interface)
- Introduction
	- In the PF component, methods are provided to create, destroy, open, and close paged files, to scan through the pages of a given file, to read a specific page of a given file, to add and delete pages of a given file, and to obtain and release pages for scratch use.
	- ***Positive nonzero return codes indicate non-error exception conditions (such as reaching the end of a file) or errors from which the system can recover or exit gracefully (such as trying to close an unopened file). Negative nonzero return codes indicate errors from which the system cannot recover***.
- The Buffer Pool of Pages
	- Accessing data on a page of a file requires first reading the page into a *buffer poll* in main memory, then manipulating its data there.
	- The PF component uses a Least-Recently-Used(LRU) page replacement policy.
	- It is import not to leave pages pinned in memory unnecessarily.
- Page Numbers
	- Pages in a file are identified by *page numbers*, which correspond to their location within the file on disk. The PF component reallocates previously allocated pages using a LIFO(stack) algorithm that is it reallocates the most recently deleted (and not reallocated) page.
	- When you scan through a file by calling the `GetFirstPage` and `GetNextPage` methods (described below), you will obtain pages in their numeric order, skipping those pages that were allocated and then deleted, and ending the scan with the largest page number currently valid.
- Page Deallocation
	- PF componet itself deallocates PF file pages.
- Scratch Pages
	- The PF component includes methods for allocating and disposing of scratch pages (memory blocks) in the buffer pool. These blocks reside in the buffer pool and are handled by the buffer manager, but they are not associated with a particular file.
- Miscellaneous Notes
	- PF_PAGE_SIZE = 4092
	- PF_BUFFER_SIZE = 40, the number of pages in the buffer pool.
	- The PF component handles all memory management for the buffer pool.
- PF Interface
  consists of 3 classes: PF_Manager, PF_FileHandle and PF_PageHandle.
	- PF_Manager Class
	  handles the creation, deletion, opening, and closing of paged files, along with the allocation and disposal of scratch pages.
	  ```
	  class 
	  ```
	-