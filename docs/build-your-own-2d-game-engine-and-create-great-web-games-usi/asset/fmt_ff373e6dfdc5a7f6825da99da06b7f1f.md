
## Game Level from a Scene File

The operations involved in initiating a game level from a scene file can assist in the derivation and refinement of the formal interface between the game engine and its client. With a game level defined in a scene file, the game engine must first initiate asynchronous loading, wait for the load completion, and then initialize the client for the game loop. These steps present a complete functional interface between the game engine and the client. By examining and deriving the proper support for these steps, the interface between the game engine and its client can be refined.

### The Scene File Project

This project uses the loading of a scene file as the vehicle to examine the necessary public methods for a typical game level. You can see an example of this project running in Figure [4-4](#334805_2_En_4_Chapter.xhtml#Fig4). This project appears and interacts identically to the previous project with the only difference being that the scene definition is asynchronously loaded from a file. The source code to this project is defined in the `chapter4/4.4.scene_file` folder.

![Figure 4-4 ¶ Running the Scene File project](images/334805_2_En_4_Chapter/334805_2_En_4_Fig4_HTML.jpg)

**Figure 4-4 ¶ Running the Scene File project**

The controls of the project are identical to the previous project, as follows:

*   **Right-arrow key**: Moves the white square toward the right and wraps it to the left of the game window
*   **Up-arrow key**: Rotates the white square
*   **Down-arrow key**: Increases the size of the red square and then resets the size at a threshold

The goals of the project are as follows:

*   To introduce the protocol for supporting asynchronous loading of the resources of a game
*   To develop the proper game engine support for the protocol
*   To identify and define the public interface methods for a general game level

While the parsing and loading process of a scene file is interesting to a game engine designer, the client should never need to concern themselves with these details. This project aims at developing a well-defined interface between the engine and the client. This interface will hide the complexity of the engine internal core from the client and thus avoid situations such as requiring access to the `loop` module from `MyGame` in the first project of this chapter.

#### The Scene File

Instead of hard-coding the creation of all objects to a game in the `init()` function, the information can be encoded in a file, and the file can be loaded and parsed during runtime. The advantage of such encoding in an external file is the flexibility to modify a scene without the need to change the game source code, while the disadvantages are the complexity and time required for loading and parsing. In general, the importance of flexibility dictates that most game engines support the loading of game scenes from a file.

Objects in a game scene can be defined in many ways. The key decision factors are that the format can properly describe the game objects and be easily parsed. Extensible Markup Language (XML) is well suited to serve as the encoding scheme for scene files.

#### Define an XML Resource Module

In order to support an XML-encoded scene file, you first need to expand the engine to support the asynchronous loading of an XML file resource. Similar to the text resource module, an XML resource module should also be based on the `resource_map`: store the loaded XML content in `mMap` of the `resource_map`, and define the specifics for decoding and parsing for the calling of the `loadDecodeParse()` function of the `resource_map`.

1.  **Define a new file in the `src/engine/resources` folder and name it `xml.js`. Edit this file and import the core resource management functionality from the `resource_map`.**

    ```
    "use strict"
    import * as map from "../core/resource_map.js";
    // functions from resource_map
    let unload = map.unload;
    let has = map.has;
    let get = map.get;
    ```

2.  **Instantiate an XML `DOMParser`, define the decoding and parsing functions, and call the `loadDecodeParse()` function of the `resource_map` with the corresponding parameters to initiate the loading of the XML file:**

    ```
    let mParser = new DOMParser();
    function decodeXML(data) {
        return data.text();
    }
    function parseXML(text) {
        return mParser.parseFromString(text, "text/xml");
    }
    function load(path) {
        return map.loadDecodeParse(path, decodeXML, parseXML);
    }
    ```

3.  **Remember to export the defined functionality:**

    ```
    export {has, get, load, unload}
    ```

4.  **Lastly, remember to export the defined functionality for the client in the `index.js`:**

    ```
    import * as xml from "./resources/xml.js";
    // ... identical to previous code ...
    export default {
        // resource support
        text,
        xml,
        // ... identical to previous code ...
    }
    ```

The newly defined `xml` module can be conveniently accessed by the client and used in a similar fashion as the `text` module in loading external XML-encoded text files.

> **Note**
>
> The JavaScript `DOMParser` provides the ability to parse XML or HTML text strings.

#### Modify the Engine to Integrate Client Resource Loading

The scene file is an external resource that is being loaded by the client. With asynchronous operations, the game engine must stop and wait for the completion of the load process before it can initialize the game. This is because the game initialization will likely require the loaded resources.

##### Coordinate Client Load and Engine Wait in the Loop Module

Since all resource loading and storage are based on the same `resource_map`, the client issuing of the load requests and the engine waiting for the load completions can be coordinated in the `loop.start()` function as follows:

```
async function start(scene) {
    if (mLoopRunning) {
        throw new Error("loop already running")
    }
    mCurrentScene = scene;
    mCurrentScene.load();
    // Wait for any async requests before game-load
    await map.waitOnPromises();
    mCurrentScene.init();
    mPrevTime = performance.now();
    mLagTime = 0.0;
    mLoopRunning = true;
    mFrameID = requestAnimationFrame(loopOnce);
}
```

Note that this function is exactly two lines different from the previous project—`mCurrentScene` is assigned a reference to the parameter, and the client's `load()` function is called before the engine waits for the completion of all asynchronous loading operations.

##### Derive a Public Interface for the Client

Though slightly involved, the details of XML-parsing specifics are less important than the fact that XML files can now be loaded. It is now possible to use the asynchronous loading of an external resource to examine the required public methods for interfacing a game level to the game engine.

##### Public Methods of MyGame

While the game engine is designed to facilitate the building of games, the actual state of a game is specific to each individual client. In general, there is no way for the engine to anticipate the required operations to initialize, update, or draw any particular game. For this reason, such operations are defined to be part of the public interface between the game engine and the client. At this point, it is established that `MyGame` should define the following:

*   `constructor()`: For declaring variables and defining constants.
