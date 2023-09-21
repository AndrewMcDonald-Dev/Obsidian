
How to build utility functions on a Linked List.

```java
public int size(Node<E> n){
	if (n==null) {
		return 0;
	}

	return this.size(n.link) + 1;
})
```


```java
public boolean push(E data) {
	if head == null {
		head == new Node<E>(data);
	} else {
		newHead == new Node<E>(data);
		newHead.link == head;
		head == newHead
	}
}
```