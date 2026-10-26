2. Next, we sort by decreasing order of vertices, and start by coloring red:

![](bbox=[0.039, 0.108, 0.581, 0.379])

**Decreasing order of vertices by degree:**  
4, 5, 2, 6, 8, 1, 3, 7

+   **Vertex colors:**
+   1: Red  
+   2: Cannot be red, connected to 4  
+   3: Cannot be red, connected to 4  
+   4: Red  
+   5: Cannot be red, connected to 4  
+   6: Cannot be red, connected to 4  
+   7: Red  
+   8: Cannot be red, connected to 4

Figure 5.25: Coloring red

3. In the next round, we start coloring blue:

![](bbox=[0.047, 0.469, 0.581, 0.735])

**Decreasing order of vertices by degree:**  
4, 5, 2, 6, 8, 1, 3, 7

+   **Vertex colors:**
+   1: Red  
+   2: Cannot be blue, connected to 5  
+   3: Blue  
+   4: Red  
+   5: Blue  
+   6: Blue  
+   7: Red  
+   8: Cannot be red, connected to 5, 6

Figure 5.26: Coloring blue