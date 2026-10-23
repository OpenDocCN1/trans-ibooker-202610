```
// Get a reference to the three properties in the
// ObjectiveTrigger.
var questProperty =
    property.FindPropertyRelative("quest");
var statusProperty =
    property.FindPropertyRelative("statusToApply");
var objectiveNumberProperty =
    property.FindPropertyRelative("objectiveNumber");

// We want to display three controls:
// - An Object field for dropping a Quest object into
// - A pop-up field for selecting a Quest.Status from
// - A pop-up field for selecting the specific objective from;
//   it should show the name of the objective.
//
// If no Quest has been specified, or if the Quest has no
// objectives, the objective pop-up should be empty and
// disabled.

// Calculate the rectangles in which we're displaying.
var lineSpacing = 2;

// Calculate the rectangle for the first line
var firstLinePosition = position;

firstLinePosition.height =
    base.GetPropertyHeight(questProperty, label);

// And for the second line (same as the first line, but shifted
// down one line)
var secondLinePosition = position;
secondLinePosition.y = firstLinePosition.y +
    firstLinePosition.height + lineSpacing;
secondLinePosition.height =
    base.GetPropertyHeight(statusProperty, label);

// Repeat for the third line (same as the second line, but
// shifted down)
var thirdLinePosition = position;
thirdLinePosition.y = secondLinePosition.y +
    secondLinePosition.height + lineSpacing;
thirdLinePosition.height =
    base.GetPropertyHeight(objectiveNumberProperty, label);

// Draw the quest and status properties, using the automatic
// property fields
EditorGUI.PropertyField(firstLinePosition, questProperty,
    new GUIContent("Quest"));
EditorGUI.PropertyField(secondLinePosition, statusProperty,
```