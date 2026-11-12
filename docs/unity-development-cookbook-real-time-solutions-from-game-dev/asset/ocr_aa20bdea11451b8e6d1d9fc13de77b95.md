Materials and shaders designed for one render pipeline won’t work for other pipelines, because they rely on features provided by the render pipeline they were designed for. If you try to use a material designed for the Built-In Render Pipeline in a project using the Universal Render Pipeline, for example, it will render a bright, solid purple color that indicates an error.

In addition to creating a new project that’s already using an SRP, you can also adapt an existing project to do it:

+   1. Open the Window menu, and choose Package Manager.
+   
+   2. In the drop-down menu at the top left of the window, choose Unity Registry.
+   
+   3. Select the Universal RP package from the package list, and click Install. Unity will download and install the package into your project.
+   
+   4. Open the Assets menu, and choose Create → Rendering → URP Asset (with Universal Renderer). A new pipeline asset will be created; this asset contains the rendering settings used for your project, along with a Renderer.
+   
+   5. Open the Project Settings window by opening the Edit menu and choosing Project Settings.
+   
+   6. Select the Graphics settings.
+   
+   7. Drag the Universal Pipeline Asset you created into the Scriptable Render Pipeline Settings slot.

Note that when you start using an SRP, you’ll need to modify your materials to use shaders that are designed for the new pipeline. To do this, select your Materials in the Project tab, open the Edit menu, and choose Rendering → Materials → Convert Selected Built-In Materials to URP Materials.