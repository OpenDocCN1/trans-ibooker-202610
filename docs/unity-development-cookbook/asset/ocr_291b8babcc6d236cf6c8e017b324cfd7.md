```
"z": 0.0,
"w": 1.0
},
"localScale": {
"x": 1.0,
"y": 1.0,
"z": 1.0
}
},
],

// "scenes" contains the list of scenes open when the game was saved.
"scenes": [
"SavingLoadingGameState",
"SavingDemoExtraScene"
],

// "activeScene" is the name of the currently active scene. The
// active scene is the one into which new objects are instantiated,
// and controls the lighting settings for the game.
"activeScene": "SavingLoadingGameState"
}
```

# Downloading LitJSON

To read and write our JSON, we'll be using a third-party open source library called LitJSON.

![](bbox=[0.16, 0.52, 0.23, 0.62])

Unity provides a tool for working with JSON—the JsonUtility class—but it doesn't meet our needs for saving and loading, because it sits on top of Unity's existing serialization tool. That's not to say that it's entirely useless—in fact, we use it for part of this serialization system to save us some work—but it's not a general-purpose JSON library.

To install LitJSON, follow these steps:

+   1. Make a new folder in your Assets directory called LitJSON.
+   2. In your browser, go to the GitHub page for the latest release of LitJSON.
+   3. Download the source code as a ZIP file.
+   4. Unzip the ZIP file. It will contain a folder called src.
+   5. Copy the contents of the src directory into the LitJSON folder you created.