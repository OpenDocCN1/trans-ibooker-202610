# 2.2 Running Code at Specific Times in a Script's (or Game Object's) Life

# Problem

You want to run code at specific times during the life cycle of a script.

# Solution

Unity calls different methods on your MonoBehaviours at different times:

# Start

The Start method is called once per object for each script, on the first frame that the script is active. If the script is attached to the object when the game starts, then Start is called on the first frame.

# Update

The Update method is called once per frame for each script on each active object, before the camera renders the scene. Because it’s called every frame, it’s your best opportunity to do work that happens continuously, like movement.

# Awake

The Awake method, like the Start method, is called when the script becomes active. Unity guarantees that all Awake methods will complete before any Start methods begin; this means that the Awake method is the best time to set up references between scripts (using GetComponent and FindObjectOfType, discussed in Recipes 2.4 and 2.5), and Start is the best time to start accessing data from those references.

# LateUpdate