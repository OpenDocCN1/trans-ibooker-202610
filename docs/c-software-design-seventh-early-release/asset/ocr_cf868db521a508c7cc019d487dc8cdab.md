High-level
(stable, low dependencies)

Low-level
(volatile, malleable, high dependencies)

Architectural
Boundary

Editor

VimModePlugin

Figure 2-5. Broken plugin architecture: the high-level Editor class depends on the low-level VimModePlugin class.

Of course that should not happen. In the given Editor example it is certainly not a good idea to make the Editor class depend on all the concrete plugins. Instead, you should reach for an abstraction, for instance in the form of a Plugin base class. The Plugin class now represents the abstraction for all kinds of plugins. However, it does not make sense to