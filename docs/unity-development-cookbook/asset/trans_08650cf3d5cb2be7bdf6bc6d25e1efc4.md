

现在我们来设置存储任务信息的对象类型。创建一个新的 C# 脚本命名为 Quest.cs，并添加以下代码：

```
#if UNITY_EDITOR
using UnityEditor;
#endif

// A Quest stores information about a quest: its name and its
// objectives.

// CreateAssetMenu makes the Create Asset menu contain an entry that
// creates a new Quest asset.
[CreateAssetMenu(fileName = "New Quest", menuName = "Quest", order = 100)]
public class Quest : ScriptableObject
{
    // Represents the status of objectives and quests
    public enum Status {
        NotYetComplete, // the objective or quest has not yet been
                         // completed
        Complete,        // the objective or quest has been successfully
                         // completed
        Failed           // the objective or quest has failed
    }

    // The name of the quest
    public string questName;

    // The list of objectives that form this quest
    public List<Objective> objectives;

    // Objectives are the specific tasks that make up a quest.
    [System.Serializable]
    public class Objective
    {
        // The visible name that's shown to the player
        public string name = "New Objective";

        // If true, this objective doesn't need to be complete for the
        // quest to be considered complete.
        public bool optional = false;

        // If false, the objective will not be shown to the user if
        // it's not yet complete. (It will be shown if it's Complete
        // or Failed.)
        public bool visible = true;

        // The status of the objective when the quest begins. Usually
        // this will be "not yet complete," but you might want an
        // objective that starts as Complete, and can be Failed.
        public Status initialStatus = Status.NotYetComplete;
    }
}
```

```
#if UNITY_EDITOR
// Draw a custom editor that lets you build the list of objectives.
[CustomEditor(typeof(Quest))]
public class QuestEditor : Editor {

    // Called when Unity wants to draw the Inspector for a quest.
    public override void OnInspectorGUI()
    {

        // Ensure that the object we're displaying has had any pending
        // changes done.
        serializedObject.Update();

        // Draw the name of the quest.
        EditorGUILayout.PropertyField(
            serializedObject.FindProperty("questName"),
            new GUIContent("Name")
        );

        // Draw a header for the list of objectives
        EditorGUILayout.LabelField("Objectives");

        // Get the property that contains the list of objectives
        var objectiveList = serializedObject.FindProperty("objectives");

        // Indent the objectives
        EditorGUI.indentLevel += 1;

        // For each objective in the list, draw an entry
        for (int i = 0; i < objectiveList.arraySize; i++)
        {
            // Draw a single line of controls
            EditorGUILayout.BeginHorizontal();

            // Draw the objective itself (its name and its flags)
            EditorGUILayout.PropertyField(
                objectiveList.GetArrayElementAtIndex(i),
                includeChildren: true
            );

            // Draw a button that moves the item up in the list
            if (GUILayout.Button("Up", EditorStyles.miniButtonLeft,
                GUILayout.Width(25)))
            {
                objectiveList.MoveArrayElement(i, i - 1);
            }

            // Draw a button that moves the item down in the list
```

```
if (GUILayout.Button("Down", EditorStyles.miniButtonMid,
    GUILayout.Width(40)))
{
    objectiveList.MoveArrayElement(i, i + 1);
}

// Draw a button that removes the item from the list
if (GUILayout.Button("-", EditorStyles.miniButtonRight,
    GUILayout.Width(25)))
{
    objectiveList.DeleteArrayElementAtIndex(i);
}

EditorGUILayout.EndHorizontal();
}

// Remove the indentation
EditorGUI.indentLevel -= 1;

// Draw a button that adds a new objective to the list
if (GUILayout.Button("Add Objective"))
{
    objectiveList.arraySize += 1;
}

// Save any changes
serializedObject.ApplyModifiedProperties();
}
}
#endif
```

![](img/e54c00769db669e3746fa1e50090241f_254_0.png)

有关我们用来将任务信息存储为资源 的 ScriptableObject 类的更多信息，请参阅 Recipe 2.9。

我们现在可以创建一个任务资源：

- 1. 打开 Assets 菜单，选择 Create → Quest。将新任务命名为“Click on the Blocks。”
- 2. 点击 Add Objective。将新目标命名为“Click on the red block。”打开 Visible 复选框。

- 3. 再重复此过程两次——分别针对绿色和黄色方块。让其中一个为可选，另一个为可选且不可见。
- 4. 再添加一个目标，用于蓝色方块。对于这个目标，将其名称设置为“Don’t click on the blue block。”将其 Initial Status 设置为“Complete。”

完成后，Inspector 应该看起来像图 9-6。

![](img/e54c00769db669e3746fa1e50090241f_255_0.png)

图 9-6. 任务资源的 Inspector，其中配置了多个目标

我们现在来设置管理任务的代码：

- 1. 创建一个新的 C# 脚本命名为 QuestManager.cs，并添加以下代码：

```
// Represents the player's current progress through a quest.
public class QuestStatus {

    // The underlying data object that describes the quest.
    public Quest questData;

    // The map of objective identifiers.
    public Dictionary<int, Quest.Status> objectiveStatuses;

    // The constructor. Pass a Quest to this to set it up.
    public QuestStatus(Quest questData)
    {
        // Store the quest info
        this.questData = questData;

        // Create the map of objective numbers to their status
        objectiveStatuses = new Dictionary<int, Quest.Status>();

        for (int i = 0; i < questData.objectives.Count; i += 1)
        {
            var objectiveData = questData.objectives[i];

            objectiveStatuses[i] = objectiveData.initialStatus;
        }
    }

    // Returns the state of the entire quest.
    // If all nonoptional objectives are complete, the quest is complete.
    // If any nonoptional objective is failed, the quest is failed.
    // Otherwise, the quest is not yet complete.
    public Quest.Status questStatus {
        get {

            for (int i = 0; i < questData.objectives.Count; i += 1) {

                var objectiveData = questData.objectives[i];

                // Optional objectives do not matter to the overall
                // quest status
                if (objectiveData.optional)
                    continue;

                var objectiveStatus = objectiveStatuses[i];

                // This is a mandatory objective
                if (objectiveStatus == Quest.Status.Failed)
```

```
{ // If a mandatory objective is failed, the whole // quest is failed return Quest.Status.Failed; } else if (objectiveStatus != Quest.Status.Complete) { // If a mandatory objective is not yet complete, // the whole quest is not yet complete return Quest.Status.NotYetComplete; } } // All mandatory objectives are complete, so this quest is // complete return Quest.Status.Complete; } } // Returns a string containing the list of objectives, their // statuses, and the status of the quest. public override string ToString() { var stringBuilder = new System.Text.StringBuilder(); for (int i = 0; i < questData.objectives.Count; i += 1) { // Get the objective and its status var objectiveData = questData.objectives[i]; var objectiveStatus = objectiveStatuses[i]; // Don't show hidden objectives that haven't been finished if (objectiveData.visible == false && objectiveStatus == Quest.Status.NotYetComplete) { continue; } // If this objective is optional, display "(Optional)" after // its name if (objectiveData.optional) { stringBuilder.AppendFormat("{0} (Optional) - {1}\n", objectiveData.name, objectiveStatus.ToString()); } else { stringBuilder.AppendFormat("{0} - {1}\n",
