
## Continuing from the Previous Project

Open the `simple_shader.js` file and edit the `loadAndCompileShader()` function to receive a file path instead of an HTML ID:

```javascript
function loadAndCompileShader(filePath, shaderType)
```

Within the `loadAndCompileShader()` function, replace the HTML element retrieval code in step A with the following `XMLHttpRequest` to load a file:

```javascript
let xmlReq, shaderSource = null, compiledShader = null;
let gl = core.getGL();
// Step A: Request the text from the given file location.
xmlReq = new XMLHttpRequest();
xmlReq.open('GET', filePath, false);
try {
    xmlReq.send();
} catch (error) {
    throw new Error("Failed to load shader: "
        + filePath
        + " [Hint: you cannot double click to run this project. "
        + "The index.html file must be loaded by a web-server.]");
    return null;
}
shaderSource = xmlReq.responseText;
if (shaderSource === null) {
    throw new Error("WARNING: Loading of:" + filePath + " Failed!");
    return null;
}
```

Notice that the file loading will occur synchronously where the web browser will actually stop and wait for the completion of the `xmlReq.open()` function to return with the content of the opened file. If the file should be missing, the opening operation will fail, and the response text will be `null`.

The synchronized “stop and wait” for the completion of `xmlReq.open()` function is inefficient and may result in slow loading of the web page. This shortcoming will be addressed in Chapter 4 when you learn about the asynchronous loading of game resources.

> **Note**
>
> The `XMLHttpRequest()` object requires a running web server to fulfill the HTTP get request. This means you will be able to test this project from within VS Code with the installed “Go Live” extension. However, unless there is a web server running on your machine, you will not be able to run this project by double-clicking the `index.html` file directly. This is because there is no server to fulfill the HTTP get requests and the GLSL shader loading will fail.

With this modification, the `SimpleShader` constructor can now be modified to receive and forward file paths to the `loadAndCompileShader()` function instead of the HTML element IDs.

## Extracting Shaders into Their Own Files

The following steps retrieve the source code of the vertex and fragment shaders from the `index.html` file and create separate files for storing them:

1. Create a new folder that will contain all of the GLSL shader source code files in the `src` folder, and name it `glsl_shaders`, as illustrated in Figure 2-10.

   ![Figure 2-10 ¶ Creating the glsl_shaders folder](images/334805_2_En_2_Chapter/334805_2_En_2_Fig10_HTML.jpg)

   **Figure 2-10 ¶ Creating the glsl_shaders folder**

2. Create two new text files within the `glsl_shaders` folder, and name them `simple_vs.glsl` and `white_fs.glsl` for simple vertex shader and white fragment shader.

   > **Note**
   >
   > All GLSL shader source code files will end with the `.glsl` extension. The `vs` in the shader file names signifies that the file contains a vertex shader, while `fs` signifies a fragment shader.

3. Create the GLSL vertex shader source code by editing `simple_vs.glsl` and pasting the vertex shader code in the `index.html` file from the previous project:

   ```glsl
   attribute vec3 aVertexPosition;  // Vertex shader expects one position
   void main(void) {
       // Convert the vec3 into vec4 for scan conversion and
       // assign to gl_Position to pass the vertex to the fragment shader
       gl_Position = vec4(aVertexPosition, 1.0);
   }
   ```

4. Create the GLSL fragment shader source code by editing `white_fs.glsl` and pasting the fragment shader code in the `index.html` file from the previous project:

   ```glsl
   precision mediump float; // precision for float computation
   void main(void) {
       // for every pixel called (within the square) sets
       // constant color white with alpha-channel value of 1.0
       gl_FragColor = vec4(1.0, 1.0, 1.0, 1.0);
   }
   ```

## Cleaning Up HTML Code

With vertex and fragment shaders being stored in separate files, it is now possible to clean up the `index.html` file such that it contains only HTML code:

1. Remove all the GLSL shader code from `index.html`, such that this file becomes as follows:

   ```html
   <!DOCTYPE html>
   <html>
       <head>
           <title>Example 2.5: The Shader Source File Project</title>
           <link rel ="icon" type ="image/x-icon" href="./favicon.png">
           <!-- there are javascript source code contained in
                the external source files
           -->
           <!-- Client game code -->
           <script type="module" src="./src/my_game/my_game.js"></script>
       </head>
       <body>
           <canvas id="GLCanvas" width="640" height="480">
               <!-- GLCanvas is the area we will draw in: a 640x480 area -->
               Your browser does not support the HTML5 canvas.
               <!-- this message will show only if WebGL clearing failed -->
           </canvas>
       </body>
   </html>
   ```

   Notice that `index.html` no longer contains any GLSL shader code and only a single reference to JavaScript code. With this organization, the `index.html` file can properly be considered as representing the web page where you do not need to edit this file to modify the shaders from now on.

2. Modify the `createShader()` function in `core.js` to load the shader files instead of HTML element IDs:

   ```javascript
   function createShader() {
       mShader = new SimpleShader(
           "src/glsl_shaders/simple_vs.glsl", // Path to VertexShader
           "src/glsl_shaders/white_fs.glsl"); // Path to FragmentShader
   }
   ```

## Source Code Organization

The separation of logical components in the engine source code has progressed to the following state:

- **index.html**: This is the file that contains the HTML code that defines the canvas on the web page for the game and loads the source code for your game.
- **src/glsl_shaders**: This is the folder that contains all the GLSL shader source code files that draws the elements of your game.
- **src/engine**: This is the folder that contains all the source code files for your game engine.
- **src/my_game**: This is the client folder that contains the source code for the actual game.

## Changing the Shader and Controlling the Color

With GLSL shaders being stored in separate source code files, it is now possible to edit or replace the shaders with relatively minor changes to the rest of the source code. The next project demonstrates this convenience by replacing the restrictive constant white color fragment shader, `white_fs.glsl`, with a shader that can be parameterized to draw with any color.

### The Parameterized Fragment Shader Project

This project replaces `white_fs.glsl` with a `simple_fs.glsl` that supports drawing with any color. Figure 2-11 shows the output of running the Parameterized Fragment Shader project; notice that a red square replaces the white square from the previous projects. The source code for this project is defined in the `chapter2/2.6.parameterized_fragment_shader` folder.

![Figure 2-11 ¶ Running the Parameterized Fragment Shader project](images/334805_2_En_2_Chapter/334805_2_En_2_Fig11_HTML.jpg)

**Figure 2-11 ¶ Running the Parameterized Fragment Shader project**

The goals of the project are as follows:

- To gain experience with creating a GLSL shader in the source code structure
- To learn about the `uniform` variable and define a fragment shader with the color parameter

### Defining the simple_fs.glsl Fragment Shader
