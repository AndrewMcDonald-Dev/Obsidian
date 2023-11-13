Code for implementation of B-Tree

What data structure implements B-Tree easily?

```java
class BTreeNode {
	int n;
	int[] key = new int[MAX_KEYS];
	BTreeNode[] children = new Node[MAX_CHILDREN];
	boolean leaf;
}
```
```rust
struct BTreeNode {
	keys: Vec<i32>
	children: Vec<&BTreeNode>
}
```