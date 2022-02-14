A general idea of **LSM-Tree** is to split data across the memory and disk. Memory data points to the structures on disk. If key is not found in memory, it is looked up on the corresponding segments on disk. If in-memory data exceeds threshold - part of it is moved to the disk.

**SSTable** is a substructure of **LSM-Tree** which holds ordered key-value pairs.

![[DB_Idx_SSTable.png]]
If segment is full, we write data to the new segment file. Merging segments - just repeatedly look for the lowest entry in every segment and insert it into a merged segment

There is no need to keep index of all the keys in memory. See example below:
![[DB_Idx_SSTable_with_sparsed_index.png]]
Sparsed in-memory index is used to look for the segment that might contain the needed key.
This approach is called *LSM-Tree* and it is working as follows:
-   When a write comes in, add it to an in-memory balanced tree data structure
-   When the memtable gets bigger than some threshold - write it out to disk as an SSTable file
-   In order to serve a read request, first try to find the key in the memtable, then in the most recent on-disk segment
-   From time to time, run a merging and compaction process in the background to combine segment files and to discard overwritten or deleted values
-   Data in segments located in file system is sorted using red-black or AVL trees

One important problem in the scheme - if database crashes, the one which are in memory but not yet on the disk - are lost. We can write an unsorted log to backup in case db crashes.

Bloom filters data structure might be used in order to check presense of a key in the storage. Because keys that does not exist will take a lot of lookups: 
memtable -> sstables -> older sstables -> so on