# Table 24-7. Key Members of the FrameworkElement Type

| Members | Meaning in Life |
|---|---|
| ActualHeight, ActualWidth, MaxHeight, MaxWidth, MinHeight, MinWidth, Height, Width | These properties control the size of the derived type. |
| ContextMenu | Gets or sets the pop-up menu associated with the derived type. |
| Cursor | Gets or sets the mouse cursor associated with the derived type. |
| HorizontalAlignment, VerticalAlignment | Gets or sets how the type is positioned within a container (such as a panel or list box). |
| Name | Allows to you assign a name to the type in order to access its functionality in a code file. |
| Resources | Provides access to any resources defined by the type (see Chapter 29 for an examination of the WPF resource system). |
| ToolTip | Gets or sets the tooltip associated with the derived type. |

# The Role of System.Windows.UIElement

Of all the types within a Window's inheritance chain, the UIElement base class provides the greatest amount of functionality. The key task of UIElement is to provide the derived type with numerous events to allow the derived type to receive focus and process input requests. For example, this class provides numerous events to account for drag-and-drop operations, mouse movement, keyboard input, stylus input, and touch.

Chapter 25 digs into the WPF event model in detail; however, many of the core events will look quite familiar (MouseMove, KeyUp, MouseDown, MouseEnter, MouseLeave, etc.). In addition to defining dozens of events, this parent class provides several properties to account for control focus, enabled state, visibility, and hit-testing logic, as shown in Table 24-8.

# Table 24-8. Key Members of the UIElement Type

| Members | Meaning in Life |
|---|---|
| Focusable, IsFocused | These properties allow you to set focus on a given derived type. |
| IsEnabled | This property allows you to control whether a given derived type is enabled or disabled. |
| IsMouseDirectlyOver, IsMouseOver | These properties provide a simple way to perform hit-testing logic. |
| IsVisible, Visibility | These properties allow you to work with the visibility setting of a derived type. |
| RenderTransform | This property allows you to establish a transformation that will be used to render the derived type. |