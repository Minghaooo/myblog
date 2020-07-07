---
title: CS400-03-BTree
date: 2020-07-05 21:15:42
tags: JAVA
---

<!--more-->
### Intro
* each node has one key and up to two children. 
* A B-tree with order K is a tree where nodes can have up to K-1 keys and up to K children. 
The order is the maximum number of children a node can have. 
Ex: In a B-tree with order 4, a nodes can have 1, 2, or 3 keys, and up to 4 children. 

Order 是每个node 可以拥有的最大 children 的个数。

### Insertion

Given a new key, a 2-3-4 tree insert operation inserts the new key in the proper location such that all 2-3-4 tree properties are preserved. 
New keys are always inserted into leaf nodes in a 2-3-4 tree. Insertion returns the leaf node where the key was inserted, or null if the key was already in the tree.

An important operation during insertion is the split operation, which is done on every full node encountered during insertion traversal. 
The split operation moves the middle key from a child node into the child's parent node. 
The first and last keys in the child node are moved into two separate nodes. The split operation returns the parent node that received the middle key from the child.

核心思想是: the middle value is moved up into the parent node.

Splitting an internal node allocates 2 new nodes, each with a single key, and the middle key from the split node moves up into the parent node. 
Splitting the root node allocates 3 new nodes, each with a single key, and the root of the tree becomes a new node with a single key.

插入1个 node 有三种情况:
1. New key equals an existing key in node: No insertion takes place, and the node is not altered.
2. New key is < node's first key : Existing keys in node are shifted right, and the new key becomes node's first key.
3. Node has only 1 key or new key is < node's middle key: Node's middle key , if present, becomes last key, and new key becomes node's middle key.
4. None of the above : New key becomes node's last key.

### 2-3-4 tree rotations and fusion

#### Rotation
Removing an item from a 2-3-4 tree may require rearranging keys to maintain tree properties. 
A rotation is a rearrangement of keys between 3 nodes that maintains all 2-3-4 tree properties in the process. 
The 2-3-4 tree removal algorithm uses rotations to transfer keys between sibling nodes. 
A right rotation on a node causes the node to lose one key and the node's right sibling to gain one key. 
A left rotation on a node causes the node to lose one key and the node's left sibling to gain one key.


The rotation algorithm operates on a node, causing a net decrease of 1 key in that node. The key removed from the node moves up into the parent node, displacing a key in the parent that is moved to a sibling. No new nodes are allocated, nor existing nodes deallocated during rotation. The code simply copies key and child pointers.

#### Fusion

When rearranging values in a 2-3-4 tree during deletions, rotations are not an option for nodes that do not have a sibling with 2 or more keys. 
Fusion provides an additional option for increasing the number of keys in a node. 
A fusion is a combination of 3 keys: 2 from adjacent sibling nodes that have 1 key each, and a third from the parent of the siblings.

Fusion is the inverse operation of a split. The key taken from the parent node must be the key that is between the 2 adjacent siblings. The parent node must have at least 2 keys, with the exception of the root.

Fusion of the root node is a special case that happens only when the root and the root's 2 children each have 1 key. In this case, the 3 keys from the 3 nodes are combined into a single node that becomes the new root node.

#### Non-root fusion

For the non-root case, fusion operates on 2 adjacent siblings that each have 1 key. The key in the parent node that is between the 2 adjacent siblings is combined with the 2 keys from the two siblings to make a single, fused node. The parent node must have at least 2 keys.

In the fusion algorithm below, the BTreeGetKeyIndex function returns an integer in the range [0,2] that indicates the index of the key within the node. The  BTreeSetChild functions sets the left, middle1, middle2, or right child pointer based on an index value of 0, 1, 2, or 3, respectively.

### Merge algorithm

A B-Tree merge operates on a node with 1 key and increases the node's keys to 2 or 3 using either a rotation or fusion. 
A node's 2 adjacent siblings are checked first during a merge, and if either has 2 or more keys, a key is transferred via a rotation. 
Such a rotation increases the number of keys in the merged node from 1 to 2. 
If all adjacent siblings of the node being merged have 1 key, then fusion is used to increase the number of keys in the node from 1 to 3. The merge operation can be performed on any node that has 1 key and a non-null parent node with at least 2 keys.


### Remove algorithm

Given a key, a 2-3-4 tree remove operation removes the first-found matching key, restructuring the tree to preserve all 2-3-4 tree rules. 
Each successful removal results in a key being removed from a leaf node. 
Two cases are possible when removing a key, the first being that the key resides in a leaf node, and the second being that the key resides in an internal node.

A key can only be removed from a leaf node that has 2 or more keys. 
The preemptive merge removal scheme involves increasing the number of keys in all single-key, non-root nodes encountered during traversal. 
The merging always happens before any key removal is attempted. 
Preemptive merging ensures that any leaf node encountered during removal will have 2 or more keys, allowing a key to be removed from the leaf node without violating the 2-3-4 tree rules.

To remove a key from an internal node, the key to be removed is replaced with the minimum key in the right child subtree (known as the key's successor), or the maximum key in the leftmost child subtree. 
First, the key chosen for replacement is stored in a temporary variable, then the chosen key is removed recursively, and lastly the temporary key replaces the key to be removed.

