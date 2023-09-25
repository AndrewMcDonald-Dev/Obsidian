
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
public void push(E data) {
	if head == null {
		head == new Node<E>(data);
	} else {
		newHead == new Node<E>(data);
		newHead.link == head;
		head == newHead;
	}
}
```

```java
public void append(Node<E> n, E data) {
	if (n.link == null){
		n.link = new Node<E>(data);
	}
	this.append(n.link, data);
}
```

```java
public String toString(Node<E> n){
	if (n != null) {
		String s
		if (n.link != null){
			s = n.data + ", ";
		}
		return s += this.toString(n.link);
	}
	return "";
}

public String toString() {
	String s = "[" + this.toString(head);
	s.replaceLast(", ", "");
	return s + "]";
}
```
