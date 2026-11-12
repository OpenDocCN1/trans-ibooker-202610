```
}
#endif
```

3. Update the includes at the top of the file with the following, below the existing includes:

```
#if UNITY_EDITOR
using UnityEditor;
#endif
```

This makes sure we can use the UnityEditor namespace, and the if statement ensures it's included only when this script is being run within the editor environment. For more about this syntax, see Chapter 2.

4. Within the new WallEditor class, add the following method:

```
public override void OnInspectorGUI()
{
    // Make sure that we have the latest data
    serializedObject.Update();

    EditorGUILayout.PropertyField(
        serializedObject.FindProperty("rows"));

    EditorGUILayout.PropertyField(
        serializedObject.FindProperty("columns"));

    EditorGUILayout.PropertyField(
        serializedObject.FindProperty("brickPrefab"));

    serializedObject.ApplyModifiedProperties();

    if (GUILayout.Button("Create Wall")) {
```