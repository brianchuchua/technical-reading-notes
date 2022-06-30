# Designing Data-Intensive Applications

## Chapter 1 - Reliable, Scalable, and Maintainable Applications

- Many applications are _data-intensive_ as opposed to _compute-intensive_.
- _Data systems_ often use similar pre-built tools, but the challenge is with selecting which tools and how to combine them.
- Data systems require:
  - Reliability: Work correctly in the face of adversity.
  - Scalability: Have reasonable ways of dealing with growth.
  - Maintainability: Maintainers should be able to maintain and adapt to new use cases productively.

### Reliability

- An application should be _fault-tolerant_/_resilient_, capable of working correctly even when things go wrong.
- _Faults_ differ from _failures_. _Failure_ is the system stopping. _Fault_ is a component deviating from its spec.
- Triggering faults intentionally can be very beneficial for testing robustness. See Netflix's Chaos Monkey.
- We generally prefer tolerating faults over preventing them, but sometimes that is not feasible. (Security.)
- Reliability is important even for non-critical systems -- losing family photos in an app is given as an example.

#### Hardware Faults

- Tend to be random, independent events.
- Can add physical redundancy.
- Software fault-tolerance is now preferred in the cloud -- multiple nodes with no downtimes during reboots or upgrades.

#### Software Errors

- Tend to be correlated or chain together.
- Can be systemic.
- Can lie dormant for a long time.
- Can be mitigated with good abstractions and thorough testing.

#### Human Errors

- Configuration errors are the leading cause of outages.
  - Good admin interfaces and APIs can mitigate this risk.
- Sandbox environments can separate the place where people make mistakes from the place that leads to failures.
- Thorough testing, from unit to integration tests, can also help.

### Scalability

- Load can be described using _load parameters_. What these are depends on the application.
- Twitter is used as an example. They receive 300k requests/sec for home timeline views, and up to 12k/sec tweets.
  - They used to just use a tweets table and insert into it.
  - But then switched to pushing tweets to every follower's cache for the timeline.
  - But now use a hybrid approach due to the massive fan-out of celebrity accounts.

#### Describing Performance

- Two approaches:
  1. When increasing a load parameter but keeping resources fixed, how is performance affected?
  2. When increasing a load parameter, how much do you need to increase resources to keep performance unaffected?
- Response time tends to be the most important for online applications.
- Measuring the median across multiple percentiles (50th, 95th, 99th, 99.9th) is most common.
  - High percentages are also known as _tail latencies_.
  - Some companies guarantee these with SLOs and SLAs.
  - Queueing delays are most common the cause of the worst-case scenarios.
  - When making many backend calls, the slowest one will bog down the entire operation.

#### Approaches for Coping with Load

- _Scaling-up_ vs _scaling-out_, vertical vs horizontal. Horizontal is also known as _shared-nothing_ architecture.
  - Vertical is often simpler but expensive. Hybrid approaches are best.
  - Some systems are _elastic_ and autoscale while others needs to be scaled manually.
- Databases tend to prefer to scale up.

### Maintainability

- It's good to avoid building a "legacy" system.
- We need operability, simplicity, and evolvability.

#### Operability

- Access to good metrics.
- A good DevOps team.
- Great documentation.
- Effective strategies for rollbacks and disaster recovery.

#### Simplicity

- Avoid a big ball of mud.
- _Accidental complexity_ is the thing to avoid -- complexity that has nothing to do with the problem that is being solved.
- Clean abstractions are key to keeping applications simple.

#### Evolvability

- Systems need to change all the time. Evolvability is the ability for the system to support change with minimal friction.
- Agile methodologies help, including:
  - TDD.
  - Refactoring.
- The evolvability of a system is very linked to its simplicity and abstractions.
  
## Chapter 2

- I read this during cardio. I will catch up on notes soon.

## Chapter 3 - Storage and Retrieval

- It's worth learning how databases work under the hood so you can choose the best storage engine for your application.
- Example: Some storage engines are optimized for transactional workloads and others are optimized for analytics.
- We'll be looking over SQL and "NoSQL" databaes to start.
- There are two families of storage engines: _log-structured_ and _page-oriented_.

### Data Structures That Power Your Database

- An example is given of using two bash functions to store and retrieve data from a log file.
  - It just appends a key-value pair to the file when inserting and reads the most recent entry when retrieving.
  - Performance of appending is fast.
  - Performance of reading is slow - O(n) since it has to go through all the entries in the worst case.
- Many databases use an append-only _log_ for storage.
- To get around inefficiency, some databases allow you to define an _index_ as additional metadata in order to quickly get you to what you're looking for. Think of them as signposts.
- You can have multiple indexes -- they don't affect the primary data. It's an additional structure.
- Every index slow down writes but good ones speed up reads.
  - That's why everything is not indexed by default. The application developer has to define it.

#### Hash Indexes

- Key-value pairs resemble dictionaries in most programming languages.
- Those are usually implemented as hash maps (also known as hash tables).
  - So why not use them for indexing data on disk in our databases too?
- If a data-store uses appending to a file, the simplest way to index is to:
  - Keep an in-memory _hash map_ where every key is mapped to the byte offset of where that data is in a file.
  - You can then use that hash map to jump immediately to the key you are looking for without having to traverse the entire file.
  - When you append a new key, you can update the hash map with that new key.
- This is what Bitcast (the default storage engine of Riak) does.
  - This is well suited to situations where a few keys are updated frequently.
- But how to we prevent running out of disk space since we're always only appending?
  - We can break the log into _segments_ of a specific size that we close once they get full. Subsequent writes would go to a new segment.
  - We can then perform _compaction_ on these segments, throwing away duplicate keys in the logs, keeping only the most ercent.
  - We can also merge segments after compaction. This is done in a new file.
  - This work can be done in a background thread with writes and reads still being performed as normal.
  - Once merging is complete, we switch to the new segment files and just delete the old ones.
- Each segment has its own in-memory hash table, so we check the most recent segment's first for a key, then walk through the older ones. There aren't usually many segments thanks to the merging process so this is OK.
- When implementing segments, the following has to be considered:
  - _The file format_. CSV isn't good, so a binary format that encodes the length of a string in bytes followed by the raw string is good. Doesn't need escaping.
  - _Deleting records_. You tag a record as deleted by adding a special deletion record called a _tombstone_. That record then gets deleted when a merge process is triggered.
  - _Crash recovery_. Save snapshots of the in-memory hash to disk periodically so on boot you can just load that back up without having to rebuild it.
  - Partially written records_. Use checksums to detect if a write stopped halfway.
  - _Concurrency control_. Writes are strictly sequential, so usually there is only one writer thread. Since segments are immutable and append-only, they can be read concurrently safely by multiple threads.
- Why not update files in place instead of doing an append-only log?
  - Appending and merging are sequential writes, which perform well, especially on magnetic disk hard drives.
  - Concurrency and crash recovery are easier to implement since you're never overriding a previous value.
  - Merging segments prevents file fragmentation on the hard drive.
- Hash table limitations
  - They have to fit in memory.
  - Range queries are not efficient because they have to go through all the entries in the hash map. ("Get me keys kitty00000 through kitty99999")

### Sorted String Tables (SSTables) and Log-Structured Merge-Trees (LSM-Trees)
- You can take the segment-file approach (where key-value pairs are appended and occasionally there is compaction and merging) and enhance it by:
  - Requiring that the key-value pairs are sorted by key.
  - This is called a _sorted string table_ or _SSTable_ for short.
  - This has the following benefits:
    - Merging becomes simple and efficient, similar to a _mergesort_. You read the input files side by side, comparing the keys in each, and copying the lowest key to the new output segment file. If the same key is in both source segment files, keep the most recent.
    - You no longer need to index all the keys since they're in order. You can just index one key every few kilobytes and then jump to the closest key. (This is because a few kilobytes can be scanned quickly.)
    - You can group key-value pairs together into compressed blocks before writing them since they'll need to be scanned now anyway due to keys being every few kilobytes. Saves disk space and reduces I/O bandwidth.

### Constructing and maintaing Sorted String Tables
- Since our incoming writes can come in any order and we're not appending any more, doing this in memory is an easier approach (despite the B-Tree on-disk alternative we'll learn about later).
- Use a red-black or AVL tree in memory to insert keys in any order and read them back in sorted order.
- End-to-end:
  - When a write comes in, add it to an in-memory balanced tree. This is called a _memtable_.
  - Once the _memtable_ gtes big enough, write it to disk as a Sorted String Table file. This is fast since the balanced tree keeps it sorted.
    - This file becomes the most recent segment in the database.
    - While we write to this file, we have a new _memtable_ instance handling incoming writes.
  - When getting a read, check the _memtable_ first, then the most recent on-disk SSTable segment, and so on.
  - Now and then, do a merge/compaction process on segment files to get rid of overwritten or deleted values.
- Potential problem: Database crash leads to most recent writes being lost since they were in the _memtable_ in memory and not written to disk yet.
  - Solution: Also write to an append-only log file in parallel to writing to the _memtable_. Use this append-only log to recover from crashes only. Destroy this log whenever a _memtable_ is written to disk.

### Making an LSM-tree out of SSTables
- The above is used by LevelDB and RocksDB. LevelDB is used in Riak as an alternative to Bitcask. Cassandra and HBase use similar storage engines, both inspired by Google's Bigtable paper, which created the _SStable_ and _memtable_ terms.
- This structure was described by Patrick O'Neil as a _log-structured merge-tree_ or _LSM-tree_. Storage engines that use this are called _LSM storage engines_.
- Lucene (indexing engine for Elasticsearch and Solr) uses a similar method for its _term dictionary_.

### Performance optimizations
- LSM-tree algorithm can be slow when searching keys that don't exist (since you have to go through everything)
  - Solution: Use a _bloom filter_ to speed up searches. It's a memory efficient way to tell if a key exists or not. It approximates the contents of a set.
- There are different ways to decide when and which SSTables to compact and merge. These are:
  - _size-tiered compaction_ 
    - Used by HBase and Cassandra.
    - Newer and smaller SSTables are merged into older and larger SSTagbles.
  - _leveled compaction_
    - Used by LevelDB and RocksDB and Cassandra.
    - The key range is split into smaller SSTables and older data is moved into separate "levels". This lets compacting proceed incrementally and use less disk space.
- LSM-tree advantages:
  - LSM-trees work well even with memory constraints. 
  - Range queries become efficient since keys are sorted. 
  - Disk writes are sequential so LSM-trees can have high write throughput.

### B-Trees
- The log-structured indexes above are gaining acceptance, but the most popular type of index is actually a B-tree.
- They were introduced in 1970.
- They are the standard index implementation in _almost all relational databases_ and even many nonrelational databases.
- B-trees key key-value pairs sorted by key, like SSTables.
  - That leads to efficient key-value lookups and range queries.
  - But they're very different otherwise.
- SSTables write variable-size segments and always do so sequentially.
- B-trees instead break the database down into fixed-size _blocks_ or _pages_.
  - These are 4K in size usually. Pages are read and written one at a time. (This mimicks how the hardware of a disk works.)
  - Each _page_ has an address or location, so pages can refer to one another.
    - This is all on disk, not in memory.
    - These addresses let us create a tree of pages.
  - One page is the _root_ of a B-tree -- it'll have keys and between them references to other pages.
    - You keep traveling down the tree until you get to a _leaf_ page -- it'll either have the key values inline or a reference to pages that contain the key values.
  - The number of child page references in a single page is called the _branching factor_. This is usually several hundred.
  - To update a value in a B-tree for an existing key, you find the leaf page for it, change the value, and write the page back to disk.
  - If you want to add a new key to a B-tree, you find the page that has that range of keys, and insert if it there's room, writing the page back to disk.
    - If there isn't room, you split it into two half-full pages, and update the parent page to account for the new subdivision of keys.

### Making B-trees reliable
- B-trees work by overwriting a whole page file. This works because the location of the page does not change.
  - LSM-trees, on the other hand, never modify files in place.
  - You can think of this as a hardware operation. It's pretty similar on a HDD but an SSD is more complicated.
- Some operations require multiple pages to be overwritten, including the split page scenario.
- This is dangerous becuase you can get a crash which can lead to a corrupted index with an _orphan_ page.
  - Solution: B-trees often implement a write-ahead log (WAL, also known as a _redo log_).
  - It's an append-only file where every B-tree modification has to be written to it before it's applied to pages. It's used to restore the B-tree after a crash.
- Concurrency control is required too -- what if someone is reading a B-tree in an inconsistent state?
  - Solution: They use a _latch_ (lightweight lock).
    - Log-structured approaches don't have this problem since merging is done in the background and does not interfere with reads. Old segments are swapped atomically.

### B-tree optimizations
- Optimizations have been made to B-trees over time.
  - Instead of overwriting pages and using WAL for crash recovery, LMDB and other databaes use a copy-on-write scheme. Modified pages are written to a new location and a new version of the parent pointing to it is made too.
    - This helps concurrency control.
  - We can abbreviate keys to save space -- this increases the branching factor and leads to fewer levels.
  - B-trees try to keep pages close to each other on disk.
    - LSM-trees have an easier time with this since they rewrite large segments during merges.
  - More pointers have been added to B-trees. Leaf pages may reference siblings left and right to avoid jumps back to the parent.
  - _Fractal trees_ are a variant of B-trees that borrow log-structured ideas to reduce disk seeks. Nothing to do with fractals, bad name.

### Comparing B-Trees and LSM-Trees
- In general, LSM-Trees are faster for writes and B-trees are faster for reads.
  - Reads are slower on LSM-trees because they have to check different data structures and SSTables at different stages of compaction.
- But it depends on your workload.

### Advantages of LSM-trees
- 