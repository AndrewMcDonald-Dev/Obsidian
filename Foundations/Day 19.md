```java
public int gcd(int x, int y) {
	if(x < y) {
		return gcd(y, x)
	}
	if(y == 0){
		return x
	}
	return gcd(y, x%y)
}
```