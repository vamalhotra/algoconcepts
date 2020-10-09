---
layout: post
title: Advanced Data Structures
date: 2020-01-12 13:13:07 +0000
---

# Advanced Data structures



## Fast searching in linked list

How to make search faster for linked lists? With BST, we can discard half of the nodes after one comparison with the root. With sorted arrays, we have random access and binary search. But in linked lists, we cannot have random access, so there are two ideas to make search faster in linked lists:

1. Self Organizing lists
2. Skip lists

## Self organizing lists

Self organizing lists reorder its elements based on access pattern according to locality of reference principle (Pareto principle that 80% of reads are for 20% of the elements). Self organizing lists can use different heuristics like move-to-first where any element accessed is moved to start of the list. Similarly, we have **Splay trees** which are self-organizing binary search trees that uses rotations to move any accessed key to the root.

## Skip lists

In skip lists, search, insert, deletion, indexing are O(log n) operations instead of O(n) search in linked list. So, we will use it only when there are very few insertions but lot of search operations.

It has multiple layers of linked-lists. At the bottom is the regular linked list. At each layer on top, some of the elements from the bottom layer are chosen with some probability. If probability is 0.5, then total layers will be O(log N) where N is number of elements in the bottom-most layer.

The idea of extra layers is to enable "binary-search"-like behavior. The topmost layer only has one element which is the middle element. In layer below, you can choose to search either left or right direction which has only two elements which are the middle element of (left,middle) and (middle, right). So as you move down the layers, you are basically doing the binary search which allows O(log N) searching in linked list.

## Binary Search Trees

If you can understand the following statement, then you know about it, else refer to Basic data structures article. BST can be used to sort an array by creating a tree from the array and doing in-order traversal. Since each insert takes O(log N), creation is O(N log N) and then in-order traversal in O(N).

## Forest Data Structure

Forest data structure is nothing but a collection of disjoint trees. Eg. a country is a forest consisting of each state or union territories as tree. Or consider a person in facebook which who has zero friends and no other information. That person node is not connected to any other node (person node or any other entity node) in the graph. So, it's an orphan node and a tree in itself. This tree is part of facebook graph which can be seen as forest. An empty graph is a valid forest.

## Balanced Search Trees

Balanced search trees use rotation operations to balance the tree.

### AVL Tree

### Red-Black Tree

### Splay tree

### B-Tree

It is a generalization of a binary search tree in that it allows a node to have more than two children. It is commonly used in databases and file system where large amounts of data is read or written. It allows search, insert and deletion in O(log N) time. 

#### Searching in B-Tree

Let's assume a sorted array of 1 M integers. To search whether a number exists, we can use binary search which will take log2(1M) = 20 or 2^20 = 1M since 2^10 = 1024 = 1K i.e. 20 search operations. 

20 reads are too slow in case of large databases which are stored on file which translates to 20 disk reads. Practically, it does not 20 disk reads since in each disk read, we read a block which will contains multiple numbers. If a block contains say 100 numbers, then last 6 search operations (2^6 = 64) will be inside those 100 numbers, so actual disk reads will be ~14. Can we do better?

We can do better by creating an auxiliary or sparse index which will contain first element of each block. A block contains 100 numbers, so for 1M numbers, blocks are 10K. So, our sparse index contains 10K numbers. We can search this aux index which will tell us the block to search for and that block can be read with just single disk read operation. How many reads to search aux index? 10000 = 2^14 and last 6 reads will be in same block, so effectively 8 disk reads + 1 final disk read to fetch the block, totaling 9 reads which is less than 14 previously. Can we do better?

We can do better by creating aux-index for aux-index i.e. for 10000 numbers, we will divide it into 100 blocks and store first number of each block. This aux-aux-index contains only 100 numbers which fits into single disk block and needs only 1 read access. This read will give us 100 blocks of aux-index from which we can find one block and read it. This will give us an index into block of actual file from which we can read actual numbers. So, the problem is solved in 3 reads instead of 9 or 14.

To recap, 1M numbers are stored on disk in block size of 100 numbers. So, partitions are 10K. Call it an array P0 of 10K elements.

We store first number of each partition i.e. total 10K numbers separately. This storage is again on disk in block size of 100 numbers. So, partitions are now 100. Call it an array P1 of 100 elements

We store this partition on disk and it fits into single block. Call it an array P2 of 1 element

While searching for a number, we first read array P2 and find the 'index' in P1. Then we read P1[index] and find the 'index' in P0. Finally, we read P0[index] to get 100 numbers. We search among these 100 numbers to see whether our element exists or not. This is achieved in just 3 disk reads.

#### Insertion and deletion in B-Tree

Instead of densely packing all the records in a block, the block can have some free space for subsequent insertions. In above example, instead of fully packing each block with 100 integers, we keep it half-full.  So, future insertions do not need any reorganization. In case a block gets full, space is found in adjacent blocks or a block is split into two blocks. 

In case of deletion, a record(number) is just marked as deleted and blocks are kept as-is.

To recap, B-Tree uses all of the techniques to be a good choice for databases and file-system.

- Keeps keys in sorted order for sequential traversing
- Uses a hierarchical index to minimize disk reads
- Uses partially full blocks for insertion/deletion
- Keep the index balanced with a recursive algorithm

 Apple's HFS+, Microsoft's NTFS and some Linux filesystems, such as Ext4, use B-trees. 

For more details on searching/insertion/deletion logic, please refer to wikipedia page [here]( https://en.wikipedia.org/wiki/B-tree)

#### How does B-Tree compares to BST?

If we use BST, it will results in a tree with large number of levels. The idea behind B-Tree is to collapse large number of BST nodes into a single large node in B-Tree so that we can make equivalent of several search steps in BST by just looking (or binary searching) the content of single large node before we need another disk read.

## Priority Queues

Priority Queue is an abstract data structure which is often implemented using binary heaps. Always remember the different priority queue and heap to sound smart, at least theoretically. 

A stack allows you to retrieve elements in LIFO order while a queue provides FIFO. A dictionary provides key-based access. Priority Queue is different from all these by allowing us to retrieve element by priority irrespective of the inserting order or its key value.

If priority of all elements in known at start-time, then we can just sort by priority and use FIFO. If new elements are inserted at run-time or if priority changes for existing elements, then we need a priority queue which will give us the element with highest priority. 

### How to implement a priority queue?

- Sorted Arrays - Retrieval is O(1) but maintaining order when new element is added or priority of existing element is changed is hard

- Binary heaps - Heaps maintain an implicit binary tree structure in an array [**not** BST, pay attention] in which key of root is smaller than all its descendants. Thus, minimum keys always exists at the root. New keys can be inserted by placing them at open leaf and percolating it upwards until it reaches its right place. This supports both insertion and extract-min in O(log N) time each.

- Bounded height priority queue - Assume we have to maintain priority queue for jobs where each job can have priority from 1-5. Here, priorities are finite set of numbers from 1 to N. In such cases, we can just keep an array of linkedlist i.e. each array index points to head of a linked list and we maintain an extra variable 'top' which tells the first non-empty index between 1 to N. To retrieve an element, we just retrieve first element from the linked-list at array index 'top'. If this list becomes empty or if new element is inserted, we ensure to keep 'top' updated. This provides O(1) insertion and find-min. To quote Steven Skiena, 

  > Bounded height priority queues are very useful in maintaining the vertices of a
  > graph sorted by degree, which is a fundamental operation in graph algorithms.
  > Still, they are not as widely known as they should be. They are usually the
  > right priority queue for any small, discrete range of keys.  

- Binary Search Trees - BST help you find min or max in O(log N) time by traversing left or right pointers until it becomes null. BST is useful for implementing Priority Queue when you need other operations. See [this]( https://www.geeksforgeeks.org/why-is-binary-heap-preferred-over-bst-for-priority-queue/ ) for comparison of BST vs Binary heap for implementation of priority queue.

- Fibonacci and pairing heaps - These are extremely useful when lot of dynamic updates to priority of element happens i.e. decrease-key operation where key priority is decreased. For example, in shortest path computations when we discover a shorter route to a vertex v than previously established.

## Trie

A trie is a tree structure where root represents null string and each edge represents a character and each leaf node represent end of string. Leaf node in trie is not a node without any children (eg. the and them) but a special flag is used inside node to denote if any string ends there. Two strings with common prefix will share edges for the prefix and branch off at first distinguishing character. Trie is useful for finding whether a given query string Q, containing q characters, exists in the set in O(q) time i.e. in q-character comparisons irrespective of how many strings actually exist in the set. 

## Suffix Trees

In its simplest form, it is just a trie of n-suffixes of a n-character string. 

While a trie can quickly help find if a string exists in a set of strings, suffix tree is useful to find substring inside a string. A substring of a string is a prefix of some suffix of the string. We can also say that a substring of a string is a suffix of some prefix of the string. 

So, if we are given all prefixes or all suffixes of a string, we can build a trie and use that to answer if a string of length N is a substring of some arbitrarily sized string S in O(N) time. However, time to build a suffix tree is O(S^2) because average length of suffix is S/2 and total suffixes are S. Time taken to insert each suffix of length S/2 needs S/2 character comparisons, so total time is S * S/2 = O(S^2).

Also, space complexity is S^2 since S/2 is average length of each suffix and there are total S suffixes.

Can we do better? 

**Collapsed Suffix Trees:** We can implement suffix tree in O(S) space instead of O(S^2) by merging the edges representing a full suffix into a single edge. This will reduce edges from O(S^2) to O(S). Also, we can reduce time-complexity of constructing this tree from O(N^2) to O(N) but such algorithms are non-trivial.

### Common applications

#### Find all occurrences of q as a substring of S?

Just as with a trie, we can walk from the root to the node Nq associated with q. The positions of all
occurrences of q in S are represented by the descendents of nq, which can be identified using a depth-first search from nq. In collapsed suffix trees, it takes O(|q| + k) time to find the k occurrences of q in S.

#### Longest substring common to a set of strings

Substrings of a string are N^2, however, suffixes of a string are only N.

We can generate all N suffixes for each of the strings and put them in a trie [suffix tree]. 

Actual approach (to revisit):  Build a single collapsed suffix tree containing all suffixes of all strings, with each leaf labeled with its original string. In the course of doing a depth-first search on this tree, we can label
each node with both the length of its common prefix and the number of distinct strings that are children of it. From this information, the best node can be selected in linear time.  

### Find the longest palindrome in S

To find the longest palindrome in a string S, build a single suffix tree containing all suffixes of S and the reversal of S, with each leaf identified by its starting position. A palindrome is defined by any node in this tree that has forward and reversed children from the same position.  

### Compute LCA of two nodes

The power of suffix trees can be further augmented by using a data structure for computing the least common ancestor (LCA) of any pair of nodes x, y in a tree in constant time, after linear-time preprocessing of the tree. The least common ancestor of two nodes in a suffix tree or trie defines the node representing the longest common prefix of the two associated strings. That we can answer such queries in constant time is amazing, and proves useful as a building block for many other algorithms.

> Remember, Substring means high possibility of suffix trees

## Suffix Arrays

It is a sorted array of all suffixes of a string. It is capable of doing all things that can be done by suffix trees.

We only need to store the starting position of suffix in the array and sort the array based on actual suffix value. For visual representation, please see [wikipedia article]( https://en.wikipedia.org/wiki/Suffix_array )

### Find all occurrences of a substring in a string

Every occurence of a substring is equivalent to find all suffixes that start with the given substring. Since all suffixes are sorted, all such suffixes starting with a given prefix will be lexicographically ordered and grouped together. We need two binary search - one binary search to find the start position of interval and second binary search to find the end position of interval.

## LCP Array

Longest Common prefix array is an auxiliary data structure to a suffix array.  It stores the lengths of the longest common prefixes (LCPs) between all pairs of consecutive suffixes in a sorted suffix array.

For example, if *A* := [aab, ab, abaab, b, baab] is a suffix array, the longest common prefix between *A*[1] = aab and *A*[2] = ab is *a* which has length 1, so *H*[2] = 1 in the LCP array *H*. Likewise, the LCP of *A*[2] = ab and *A*[3] = abaab is ab, so *H*[3] = 2.

The longest repeated substring problem for a string S of length n can be solved in O(n) time using both the suffix array A and the LCP array. It is sufficient to perform a linear scan through the LCP array in order to find its maximum value v_max and the corresponding index i where v_max is stored. The longest substring that occurs at least twice is then given by S[A[i], A[i]+v_{{max}}-1].

## Graphs

Graphs can be represented using adjacency lists or adjacency matrix. Use any existing graph library and see what features it provide. 

How does big graphs work like facebook graph, Microsoft/google knowledge graph which aim to represent all entities/things in the world and the relationships between them?

Well, there's no difference. Even the big graphs use adjacency lists to store edge information. Each entity is assigned a GUID. Each entity has either relationships to other entities (like "studied-at {university}", "directed-by {person}", "born in {city}") or literal properties like name or age or weight or height or date of birth. There is a notion of primary entities and secondary entities. Primary entities exist on their own while secondary entities augment primary entities eg. place-of-birth of a person. At time of serialization, each primary entity is picked and graph walk upto some defined level happens and all information of connected vertices is picked and packed along with primary entity GUID as a single row/packet. At runtime, this information is displayed and if user clicks on any linked entity, then another server call happens which returns the data/packet/row for linked entity.

### What challenges do we normally face when dealing with big graphs?

Ranking of properties: For a primary entities, which properties and related entities to be packed. Eg. if an actor has 100 movies and we have space to keep only 10 movies, which one should be kept? 

#### Ranking of sources

If 5 sources are giving conflicting info. for same property, which one should be kept and which ones should be disabled. Also, keep it in mind that 5 bad sources copying from each other can give wrong info. while a single good source can be giving correct info. Any ranking change should be flighted and measured. NDCG [Normalized discounted cumulative gain]( https://en.wikipedia.org/wiki/Discounted_cumulative_gain ) is one of the metrics used to measure ranking of algo-results but I have not seen it used in terms of measure quality of knowledge graph facts. CG (cumulative gain) is the sum of graded relevance values of all search results. NDCG penalizes lower ranking of more-relevant documents and normalized-term refers to normalization across queries and across positions in a search engine result.  

##### Anomaly detection 

###### Single-valued predicates identification

Can a person have two date of birth? No. How do we identify the single valued predicates like date-of-birth and ensure only top ranked value is kept and not remaining values. 

###### Value range identification

Similarly, can a person have height of 10 ft or 20 ft or 200 ft? No. How do we identify the sane range and disable bad data, bad data often comes because of human errors from curated sources or due to extraction issues.

###### Relationship between predicates

Can a person born in 1950 die in 1912? If one predicate value is always less than another eg. date_of_birth vs date_of_death, then one of the two values is wrong. Next step in anomaly detection is to look at the sources providing conflicting information, see if we can partition them into two sets and break it into a separate entity which is again fed to conflation pipeline which can merge the separated info. to a different entity.

#### Static vs dynamic graphs

In static graphs, no information changes once the graph is created. But all read world graphs are usually dynamic with new edges or entities being added. For dynamic graphs, all problems are classified into one of the three categories - **freshness, correctness, coverage/comprehensive-ness.** 

#### Entity triggering

For a query, which entity to trigger. This usually takes all triggered urls for a query, look the database for an entity which has any link to those urls and try to stamp the url with entity guid. At runtime, multiple urls with stamped entities are analyzed and dominant/most-stamped entity is chosen to be shown while other "less-dominant" entities might be shown as related entities.

#### Conflation

Over-conflation and under-conflation are both problems and goal is usually 100% precision as over-conflation can hurt badly due to spiraling/spreading of wrong information across the graph.

#### Source data normalization & Ingestion

Not all sources will give height as 6 ft, some might give in metres. Also, not all source will call height as "height", some might call it with different property name like "length" or "height_metres" or "person height" etc. Similarly, some sources might give lot of incorrect data, so determining quality of data is the first step and often needs extensive cleaning up of data as part of "source ingestion pipeline" to remove bad quality data.

### Gating

Gating is essential to know when some changes beyond normal standard deviation has occured and need manual verification to see if changes are valid and do auto-rollback.

### Platform

Big graphs need big platforms which can help regularly process the full data and generate serialized adjacency-list version of graph to a file and publish it to online store from where it can be queried at runtime.

### Operations in a graph

- Walks - Traversing the graph where vertices or edges can be repeated.
- Trails - Walk in which no edge is repeated, vertices can be repeated.
- Circuits - Closes trails where vertices can be repeated but edge is not repeated and circuit traverses all vertices in the graph without crossing any edge twice.

- Paths - Neither vertex nor edge is repeated.
- Cycles - Path with start and end vertex as same.

## Sets

Given a universal set of N items U1 to Un, a collection of subsets is defined. Total subsets = 2 ^ N.

It is often required to find the union or intersection of sets. In unsorted sets, it will be O(N^2) but in sorted sets, it is O(N). Also, in sorted set, we can do binary search to search an element.

Multisets allow repetition of elements.

Sets can implemented using:

### Bit vectors

An n-bit vector or array can represent any subset S on a universal set U containing n items. Bit i will be 1 if i E S, and 0 if not. Since only one bit is needed per element, bit vectors can be very space efficient for surprisingly large values of |U|. Element insertion and deletion simply flips the appropriate bit. Intersection and union are done by “and-ing” or “oring” the bits together. The only drawback of a bit vector is its performance on sparse subsets. For example, it takes O(n) time to explicitly identify all members of sparse (even empty) subset S.

### Containers or dictionaries

A subset can also be represented using a linked list, array, or dictionary containing exactly the elements in the subset. No notion of a fixed universal set is needed for such a data structure. For sparse subsets, dictionaries can be more space and time efficient than bit vectors, and easier to work with and program. For efficient union and intersection operations, it pays to keep the elements in  each subset sorted, so a linear time traversal through both subsets identifies all duplicates.

### Bloom filters

We can emulate a bit vector in the absence of a fixed universal set by hashing each subset element to an integer from 0 to n and setting the corresponding bit. Thus, bit H(e) will be 1 if e E S. Collisions leave some possibility for error under this scheme, however, because a different key might have hashed to the same position. Bloom filters use several (say k) different hash functions H1, . . . Hk, and set all k bits Hi(e) upon insertion of key e. Now e is in S only if all k bits are 1. The probability of false positives can be made arbitrarily low by increasing the number of hash functions k and table size n. With the proper constants, each subset element can be represented using a constant number of bits independent of the size of the universal set.
This hashing-based data structure is much more space-efficient than dictionaries for static subset applications that can tolerate a small probability of error. Many can. For instance, a spelling checker that left a rare random string undetected would prove no great tragedy.

#### Applications of bloom filters

- Medium uses bloom filters for recommending post to users by filtering post which have been seen by user.

- Quora implemented a shared bloom filter in the feed backend to filter out stories that people have seen before.

- The Google Chrome web browser used to use a Bloom filter to identify malicious URLs

- Google BigTable, Apache HBase and Apache Cassandra, and Postgresql use Bloom filters to reduce the disk lookups for non-existent rows or columns

  Ref source.: [GeeksForGeeks]( https://www.geeksforgeeks.org/bloom-filters-introduction-and-python-implementation/ )

### Disjoint Subsets / Union-Find datastructure

Elements of a set can be partitioned into disjoint subsets such that each element is present in only one subset. We can then perform queries like "Are two elements in same set" or "merge two subsets" or to add new elements. All these can be done in O(n) using Union-Find or merge-find or disjoint-set data structure. Union find is also crucial in finding minimum spanning tree of a graph. It is also used in keeping track of connected components of an undirected graph.

Union-Find datastructure can be implemented using Arrays where a root or representative element of set has value equal to its array index whereas all "member" element of set have array-index value equal to index of their "representative" element.

Merging of two sets is simply done by changing array-index values of all members of set to be merged with the representative index value of set with which merging happens.

## BSP Trees

Binary space partitioning is a general technique of dividing a space into two convex sets/regions until the partitioning satisfy one or more requirements. Convex sets are those where if any two random points in the region are picked and a line is drawn, then that line will lie completely <u>inside</u> the region.

## K-d trees

K-d tree is a binary tree in which each node is a k-dimensional point. Every non-leaf node represents a hyperplane that divides the space into two parts. The hyperplane direction is chosen in the following way: every node in the tree is associated with one of the *k* dimensions, with the hyperplane perpendicular to that dimension's axis. So, for example, if for a particular split the "x" axis is chosen, all points in the subtree with a smaller "x" value than the node will appear in the left subtree and all points with larger "x" value will be in the right subtree. 

### K-d tree construction

 To construct a k-d tree, one of the technique is to "cycle through the dimensions" where we continuously split in 1..K dimensions. Eg. if there are two dimensions x & y, we first split along x and then along y, then x, then y, and so on. The point at which we split is usually a median which will result in balanced tree. But if there are large no. of points, finding median is O(n), so instead small number of points are randomly chosen and it's median is used to split.

There are other techniques like "cutting along largest dimension" for constructing k-d trees.

### Applications

- K-d trees can be used for nearest-neighbor search in O(log N).
- It can also be used to find k-nearest-neighbors.
- K-d trees are also useful for range search where you're looking for all keys between a range across multiple dimensions.

