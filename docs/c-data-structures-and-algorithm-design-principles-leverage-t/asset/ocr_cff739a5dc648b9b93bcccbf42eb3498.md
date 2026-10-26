Once the positions of the houses and roads have been determined, you should produce a new version of the original map according to the following legend:

+   - Houses should be labeled with uppercase letters corresponding to the order they were given in input (that is, 0 = A, 1 = B, 2 = C and so on).
+   - Roads should be indicated with the characters |, -, \, and /, depending on their orientation. If two roads with different orientations intersect, this should be indicated with the + character.
+   - Everything else on the map should be displayed as it was originally given in the input.

# Input Format

The program should take an input in the following format:

+   - The first line contains two space-separated integers, H and W, representing the height and width of the map.
+   - The second contains a single integer, N, which is the number of houses to be built.
+   - The next H lines each contain a string of length W, representing a row on the grid. Valid locations for building roads will be marked with the "." character.
+   - The next N lines contain two integers, x and y, which are the coordinates of the houses. The final index (that is, N - 1) always represents the entry point to the community.

# Output Format

The program should output the same map given in the input, with the following additions:

+   - The positions of each house should be labeled with uppercase letters corresponding to their zero-based index, with the origin on the top left, relative to N (that is, 0 = A, 1 = B, 2 = C, and so on).
+   - The roads connecting each pair of houses should be indicated as follows:
+     - if the road's orientation is horizontal
+     | if the road's orientation is vertical
+     / or \ if the road's orientation is diagonal
+     + if any number of roads with different orientations intersect at the same point