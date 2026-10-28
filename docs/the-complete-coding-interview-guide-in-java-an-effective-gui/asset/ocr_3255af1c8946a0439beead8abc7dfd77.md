```
&& (maxHeap[right] < maxHeap[smallest])) {
    smallest = right;
}

// swap 'smallest' with 'p' and heapify
if (smallest != p) {
    swap(maxHeap, p, smallest);
    heapifyMin(maxHeap, smallest, size);
}
}

/* Helper methods */
private static int leftChild(int parentIndex) {
    return (2 * parentIndex + 1);
}

private static int rightChild(int parentIndex) {
    return (2 * parentIndex + 2);
}

// utility function to swap two indices in the array
private static void swap(int heap[], int i, int j) {
    int aux = heap[i];
    heap[i] = heap[j];
    heap[j] = aux;
}
```

The runtime of this code is O(n) with no extra space needed. The complete application is called MaxHeapToMinHeap. It also contains the conversion of a Min Binary Heap into a Max Binary Heap.