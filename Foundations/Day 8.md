
```rust
fn add_to_bst<E>(root: Node<E>, value: E) {
	if value < root.value {
		if root.left.is_some() {
			add_to_bst(root.left.unwrap(), value);
		} else {
			root.left = Some(Node<E>::new(value));		
		}
	} else {
		if root.right.is_some() {
			add_to_bst(root.right.unwrap(), value);
		} else {
			root.right = Some(Node<E>::new(value));
		}
	}
}
```