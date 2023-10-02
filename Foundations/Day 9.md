

```java
public boolean binSearch(int[] a, int key) {
	return binSearch(a, key, 0, a.length - 1);
}

public boolean binSearch(int[] a, int key, int start, int end){
	//Base case
	if (start >= end) {
		return false;
	}

	int middle = (start + end) / 2;
	if (key == a[middle]) {
		return true;
	}
	if (key < a[middle]) {
		return binSearch(a, key, start, middle - 1);
	}
	if (key > a[middle]) {
		return binSearch(a, key, middle + 1, end);
	}
}
```