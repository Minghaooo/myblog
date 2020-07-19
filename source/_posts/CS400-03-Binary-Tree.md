---
title: CS400-03-Binary-tree
date: 2020-07-05 21:15:42
tags: JAVA
---
AVL tree 是一棵树，一颗有强迫症的树，树上的每一个结点都是平衡的。
<!--more-->
AVL 是一个每一个node 都平衡过的 二叉树。
A node's balance factor is the left subtree height minus the right subtree height, which is 1, 0, or -1 in an AVL tree.

### Insert
A rotation is a local rearrangement of a BST that maintains the BST ordering property while rebalancing the tree.
旋转帮助 tree 保持其平衡状态。

插入方式        	    描述	                      旋转方式
LL	        在a的左子树根节点的左子树上插入节点而破坏平衡	右旋转
RR	        在a的右子树根节点的右子树上插入节点而破坏平衡	左旋转
LR	        在a的左子树根节点的右子树上插入节点而破坏平衡	先左旋后右旋
RL	        在a的右子树根节点的左子树上插入节点而破坏平衡	先右旋后左旋

#### right/left rotation: 

把从 root 开始，依次向下计算，找到第一个不平衡的node， 将其向右旋转，成为原本子node 的子node，然后将新形成的 子树的root 连到总root 上去。


### double rotation
如果字树的新root变成字节点而原有的左/右结点已经被占据，那么就需要两次旋转：先将子树的子树旋转，然后再将子树旋转一下。

加起来一共有四种状况：
<img src="001.png" width="70%">

上半图是原来子树的情况，下半是新插入的结点。

以下是四种插入方式，
<img src="002.png" width="70%">


### Remove




### 红黑树
主要有以下性质:

* 每个node 有两种颜色
* root 颜色一定是黑的
* 红色 node 的 children 一定是黑的
* null 是黑的
* 从一个node 到null 的路径上有相同个数的

#### Insert algorithm

假设当前 Node 的上级是parent， 然后上级的上级是grandparent， uncle 应该是 grandparent 的另一个孩子。
* node 是 toot 那就直接涂黑。
* 如果parent 是黑的，直接退出。
* 如果parent 和uncle 都是红的，那么将parent 和uncle 涂黑，grandparent 涂红。
* 如果node 是parent的右孩， 并且parent 是左孩，then rotate left at parent, update node and parent to point to parent and grandparent, respectively。
* 如果node 是左孩，parent 是右孩，then rotate right at parent, update node and parent to point to parent and grandparent, respectively, and goto step 7.
* 7: parent 变黑，grandpa 变红。
* If node is parent's left child, then rotate right at grandparent, otherwise rotate left at grandparent.

