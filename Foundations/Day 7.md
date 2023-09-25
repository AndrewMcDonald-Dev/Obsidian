Just a bunch of Binary Tree methods.

```rust
fn count_places(num: i32) -> u32 {
	if num >= 10 {
		return count_places(num / 10) + 1;
	}
	return 1
}
```