Every game object has at least one component: a Transform. The Transform component stores the position, rotation, and scale of an object, and is also responsible for keeping track of the parent of an object. You can't remove the Transform component.

On the cube, you'll find several additional components. Each of them does something different:

# MeshFilter

Loads a mesh from a file on disk for the MeshRenderer to use. (For the cube object we're adding in this recipe, the asset is supplied by Unity; most of the game objects in your games will use assets you add to the project.)

# MeshRenderer

Draws the mesh on the screen, using a Material asset to determine its appearance.

# BoxCollider

Defines the physical shape of the object in the world.

Components let you configure how they work by exposing properties that are listed in the Inspector. For example, the MeshFilter component has a single property: the Mesh that it should be using. This is an example of an object field—it's a reference to another object that's part of your project. In the case of the MeshFilter, the field can take any Mesh object; these are found in the assets you add to your project. The type of object that an object field can use is determined by the component; you can't drop any other kind of object besides a Mesh into this field, for example.

# NOTE

Object fields don't have to refet to assets—they can refer to other objects in the scene, too.