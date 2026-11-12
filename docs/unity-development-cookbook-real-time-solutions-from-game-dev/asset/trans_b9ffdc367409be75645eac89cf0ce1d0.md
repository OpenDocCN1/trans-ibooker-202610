

```csharp
// Make it one of our siblings
newCheckpoint.transform
    .SetParent(checkpoint.transform.parent, true);

// Position it as our next sibling in the Hierarchy.
// Not technically needed, and doesn't affect the game at
// all, but it looks nicer.
var nextSiblingIndex =
    checkpoint.transform.GetSiblingIndex() + 1;

newCheckpoint.transform.SetSiblingIndex(nextSiblingIndex);

// Move it slightly so that it's visibly not the same one
newCheckpoint.transform.position =
    checkpoint.transform.position + new Vector3(1, 0, 0);

// Select it, so that we can immediately start moving it
Selection.activeGameObject = newCheckpoint.gameObject;
}
```

```csharp
// Disable this button if we don't have a next checkpoint, or if
// the next checkpoint is the lap start

var disableRemoveButton = checkpoint.next == null ||
    checkpoint.next.isLapStart;
```

```csharp
using (new EditorGUI.DisabledGroupScope(disableRemoveButton)) {
    // Display a button that removes the next checkpoint
    if (GUILayout.Button("Remove Next Checkpoint"))
    {
        // Get the node that this next checkpoint was linking to
        var next = checkpoint.next.next;

// Remove the next one
DestroyImmediate(checkpoint.next.gameObject);

// Aim ourselves at what it was pointing at
checkpoint.next = next;

}
  }
    }
      }
#endif
```

- 4. 创建一个名为“Checkpoints”的新空游戏对象。
- 5. 将一个新的空游戏对象作为 Checkpoints 的子对象添加进去。将其命名为“Checkpoint”，并向其添加 Checkpoint 组件。
- 6. 将此检查点放置在赛道起点处。
- 7. 启用 Is Lap Start。
- 8. 在检查器中单击“插入检查点”。将添加并选中一个新的检查点。沿着赛道将其定位得更靠前。
- 9. 继续插入新的检查点。你也可以选择一个较早的检查点，单击“插入检查点”在两个现有点之间插入一个新检查点，或者选择前一个检查点并单击“删除下一个检查点”来删除一个检查点。
- 10. 完成赛道布置后，选择最后一个检查点，将“圈速起点”检查点（即带有黄色标记的那个）拖入其“Next”字段。

**注意**

如果检查点之间的连接出错，可以通过更改检查点上的“Next”字段来修复。

完成后，赛道应该大致如图 9-14 所示。

- 11. 将最后一个检查点定位好，使赛道的终点线正好位于第一个和最后一个检查点中间，两者应当非常接近。参见图 9-15。

![](img/4dc1688a9a64dca9b820486c73104ca2_364_0.png)

图 9-14. 围绕赛道布置的检查点

![](img/4dc1688a9a64dca9b820486c73104ca2_365_0.png)

图 9-15. 最后一个检查点，其放置位置使终点线位于第一个和最后一个检查点之间

- 12. 将车辆稍微放在“圈速起点”检查点前方。
- 13. 创建一个画布。
- 14. 创建并添加两个带有文本组件的游戏对象。
- 15. 让其中一个显示文本“Wrong Way!”，另一个显示“Lap 1。”
- 16. 创建一个名为 LapTracker.cs 的新 C# 脚本，并向其中添加以下代码：

```csharp
// We use LINQ to help figure out the start of the circuit with fewer
// lines of code. Using LINQ allocates memory, which is something we
// try to avoid, but because we only do it once (at scene start), it's
// not as bad.
using System.Linq;

public class LapTracker : MonoBehaviour {

    // The object that we're tracking as it makes laps around the
    // circuit.
    [SerializeField] Transform target = null;

    // The number of nodes in the list we're permitted to skip. This
    // prevents the player from just driving a tiny circle from the
    // start of the track to the end ("I crossed the finish line
    // three times! That means I win!"). Increase this number to permit
    // longer shortcuts. Set this to 0 to forbid any shortcuts.
    [SerializeField] int longestPermittedShortcut = 2;

    // The UI element that appears to let the player know they're
    // going the wrong way.
    [SerializeField] GameObject wrongWayIndicator;

    // A text field that displays the number of laps the player has
    // completed.
    [SerializeField] UnityEngine.UI.Text lapCounter;

    // The number of laps the player has completed.
    int lapsComplete = 0;

    // The checkpoint that the player was near most recently.
    Checkpoint lastSeenCheckpoint;

    // The list of all checkpoints on the circuit. We keep a copy of it
    // here, because we need to use this list every frame, and because
    // using FindObjectsOfType to regenerate the list every frame
    // would be slow.
    Checkpoint[] allCheckpoints;

    // The start checkpoint is the first (and hopefully only) checkpoint
    // that has isLapStart turned on.
    Checkpoint StartCheckpoint {
        get {
            // Get the checkpoint marked as the start of the lap
            return FindObjectsOfType<Checkpoint>()
                .Where(c => c.isLapStart)
                .FirstOrDefault();
        }
    }

    void Start() {
        // Ensure that the counter says "lap 1"
        UpdateLapCounter();

        // The player isn't going the wrong way at the start
        wrongWayIndicator.SetActive(false);

        // Create the list of all checkpoints which Update will make
        // use of
        allCheckpoints = FindObjectsOfType<Checkpoint>();

        // Create the circuit of connected checkpoints
        CreateCircuit();

        // Begin the race at the start of the circuit
        lastSeenCheckpoint = StartCheckpoint;
    }

    private void Update() {
        // What's the nearest checkpoint?
        var nearestCheckpoint = NearestCheckpoint();

        if (nearestCheckpoint == null) {
            // No checkpoints! Bail out.
            return;
        }

        if (nearestCheckpoint.index == lastSeenCheckpoint.index) {
            // Nothing to do; the nearest checkpoint has not changed.
        } else if (nearestCheckpoint.index > lastSeenCheckpoint.index) {

            var distance =
                nearestCheckpoint.index - lastSeenCheckpoint.index;

            if (distance > longestPermittedShortcut + 1) {
                // The player has skipped too many checkpoints.
                // Treat this as going the wrong way.
                wrongWayIndicator.SetActive(true);
            } else {
                // We are near the next checkpoint; the player is going
                // the right way.
                lastSeenCheckpoint = nearestCheckpoint;

                wrongWayIndicator.SetActive(false);
            }
        } else if (nearestCheckpoint.isLapStart &&
            lastSeenCheckpoint.next.isLapStart) {
            // If the last checkpoint we saw is the last in the circuit,
            // and our nearest is now the start of the circuit, we just
            // completed a lap!

            lastSeenCheckpoint = nearestCheckpoint;

            lapsComplete += 1;
            UpdateLapCounter();
        } else {
            // This checkpoint is lower than the last one we saw. The
            // player is going the wrong way.
            wrongWayIndicator.SetActive(true);
        }
    }

    // Calculates the nearest checkpoint to the player.
    Checkpoint NearestCheckpoint() {
        // If we don't have a list of checkpoints to use, exit
        // immediately
        if (allCheckpoints == null) {
            return null;
        }

        // Loop through the list of all checkpoints, and find the
        // nearest one to the player's position.
        Checkpoint nearestSoFar = null;
        float nearestDistanceSoFar = float.PositiveInfinity;

        for (int c = 0; c < allCheckpoints.Length; c++) {
            var checkpoint = allCheckpoints[c];
            var distance =
            (target.position - checkpoint.transform.position)
            .sqrMagnitude;

            if (distance < nearestDistanceSoFar) {
                nearestSoFar = checkpoint;
                nearestDistanceSoFar = distance;
            }
        }

        return nearestSoFar;
    }

}

// Walks the list of checkpoints, and makes sure that they all have
// an index that's one higher than the previous one (except for
// the start checkpoint)
void CreateCircuit() {
    var index = 0;

    // Start at the start of the checkpoint
    var currentCheckpoint = StartCheckpoint;

    do
    {
        // Update the index for this checkpoint
        currentCheckpoint.index = index;
        index += 1;

        // Move to the checkpoint it's pointing to
        currentCheckpoint = currentCheckpoint.next;

        // We should not reach the end of the list; that means that
        // the circuit does not form a loop
        if (currentCheckpoint == null)
        {
            Debug.LogError("The circuit is not closed!");
            return;
        }

        // loop until we reach the start again
    } while (currentCheckpoint.isLapStart == false);
}
```

