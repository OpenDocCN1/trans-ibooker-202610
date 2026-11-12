Next, we’ll handle displaying the status of the quest and its objectives:

+   1. Select the main Camera object, and add a Physics Raycaster to it.
+   2. Create a new canvas that displays the status of the quest and its
+   objectives.
+   3. Add a game object with a Text component in it.
+   4. Resize this object so that it can comfortably contain a paragraph of
+   text.

Now we’ll set up the type of object that stores information about a quest.
Create a new C# script called Quest.cs, and add the following code to it:

```
#if UNITY_EDITOR
using UnityEditor;
#endif

// A Quest stores information about a quest: its name and its
// objectives.

// CreateAssetMenu makes the Create Asset menu contain an entry that
// creates a new Quest asset.
[CreateAssetMenu(fileName = "New Quest", menuName = "Quest",
order = 100)]
public class Quest : ScriptableObject
{

    // Represents the status of objectives and quests
    public enum Status {
    NotYetComplete, // the objective or quest has not yet been
                    // completed
    Complete,       // the objective or quest has been successfully
                    // completed
    Failed          // the objective or quest has failed
    }

    // The name of the quest
    public string questName;

    // The list of objectives that form this quest
    public List<Objective> objectives;

    // Objectives are the specific tasks that make up a quest.
```