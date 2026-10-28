# Coding challenge 5 – Finding the kth number with the prime factors 3, 5, and 7

Adobe, Microsoft



Design an algorithm to find the kth number where the only prime factors are 3, 5, and 7.



Having a list of numbers whose only prime factors are 3, 5, and 7 means a list that looks as follows: 1, 3, 5, 7, 9, 15, 21, 25, and so on. Or, to be more suggestive, it can be written as follows: 1, 1*3, 1*5, 1*7, 3*3, 3*5, 3*7, 5*3, 3*3*5, 5*7, 3*3*5, 7*7, and so on.

With this suggestive representation, we can see that we can initially insert the value 1 into a list, while the rest of the elements must be computed. The simplest way to understand the algorithm for determining the rest of the elements is to look at the implementation itself, so let's see it:

```
public static int kth(int k) {
	int count3 = 0;
	int count5 = 0;
	int count7 = 0;
	List<Integer> list = new ArrayList<>();
	list.add(1);
	while (list.size() <= k + 1) {
		int m = min(min(list.get(count3) * 3, list.get(count5) * 5), list.get(count7) * 7);
		list.add(m);
		if (m == list.get(count3) * 3) {
			count3++;
		}
		if (m == list.get(count5) * 5) {
			count5++;
		}
		if (m == list.get(count7) * 7) {
			count7++;
		}
	}
	return list.get(k);
}
```