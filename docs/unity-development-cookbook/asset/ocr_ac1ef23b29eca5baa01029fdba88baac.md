```
// Create a generic cube object, which we'll make changes to and
// save as a new asset.
var cubeObject = GameObject.CreatePrimitive(PrimitiveType.Cube);

// Get the last part of the filepath, and use it as the cube's
// name
string name =
    System.IO.Path.GetFileNameWithoutExtension(ctx.assetPath);

// Next, we'll create a cube that's the right size. The default
// cube mesh is 1x1x1; we'll scale it based on the size that was
// passed in.

// Copy the default cube mesh.
var cubeMesh =
    Instantiate(cubeObject.GetComponent<MeshFilter>()
        .sharedMesh);

// Create a matrix that scales vertices by the given X, Y, and Z
// amounts.
var scaleMatrix = Matrix4x4.Scale(cubeDescription.size);

// Get a copy of the vertices in the mesh.
var vertices = cubeMesh.vertices;

// For each of these vertices, apply the scale by multiplying
// the matrix against the vertex.
for (int v = 0; v < vertices.Length; v++) {
    vertices[v] = scaleMatrix.MultiplyPoint(vertices[v]);
}

// Store these scaled vertices in the mesh.
cubeMesh.vertices = vertices;

// Tell the cube's MeshFilter to use this new mesh.
cubeObject.GetComponent<MeshFilter>().sharedMesh = cubeMesh;

// Give the mesh a name.
cubeMesh.name = name + " Mesh";

// Create a new material, using the Standard shader (which is
// the default)
var cubeMaterial = new Material(Shader.Find("Standard"));

// Apply the color that we loaded.
cubeMaterial.color = cubeDescription.color;

// Give it a name, too.
cubeMaterial.name = name + " Material";
```