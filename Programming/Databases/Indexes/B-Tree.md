The most commonly used type of index.

B-trees keep key-value pairs sorted by key, which allows efficient key-value lookups and range queries. 

B-trees break the database down into fixed-size blocks or pages, traditionally 4 KB in size. That size typically corresponds to the underlying hardware, as disks are also arranged in fixed-size blocks.

Each page can be identified using an address or location, which allows one page to refer to another—similar to a pointer, but on disk instead of in memory.
 
![[DB_Idx_B-Tree.png]]
  
 One page is designated as the root of the B-tree; whenever you want to look up a key in the index, you start here. The page contains several keys and references to child pages.
  
 Each child is responsible for a continuous range of keys, and the keys between the references indicate where the boundaries between those ranges lie.
  
Eventually we get down to a page containing individual keys (a leaf page), which either contains the value for each key inline or contains references to the pages where the values can be found.

The number of references to child pages in one page of the B-tree is called the *branching factor*. In practice, the branching factor depends on the amount of space required to store the page refer‐ ences and the range boundaries, but typically it is several hundred.

If you want to update the value for an existing key in a B-tree, you search for the leaf page containing that key, change the value in that page, and write the page back to disk.

If you want to add a new key, you need to find the page whose range encompasses the new key and add it to that page. If there isn’t enough free space in the page to accommodate the new key, it is split into two half-full pages, and the parent page is updated to account for the new subdivision of key ranges

![[DB_Idx_B-Tree_new_page.png]]

This algorithm ensures that the tree remains balanced: a B-tree with n keys always has a depth of *O(log n)*.

If the database crashes after only some of the pages have been written, you end up with a corrupted index. In order to make the database resilient to crashes, it is common for B-tree implementations to include an additional data structure on disk: a *write-ahead log*. This is an append-only file to which every B-tree modification must be written before it can be applied to the pages of the tree itself. When the database comes back up after a crash, this log is used to restore the B-tree back to a consistent state.

To protect from concurrent page change - *latches* used (lightweight locks)

**Optimizations :**

-   We can save space in pages by not storing the entire key, but abbreviating it. Especially in pages on the interior of the tree, keys only need to provide enough information to act as boundaries between key ranges. Packing more keys into a page allows the tree to have a higher branching factor, and thus fewer levels.
-   Many B- tree implementations try to lay out the tree so that leaf pages appear in sequential order on disk.
-   Each leaf page may have references to its sibling pages to the left and right, which allows scanning keys in order without jumping back to parent pages.