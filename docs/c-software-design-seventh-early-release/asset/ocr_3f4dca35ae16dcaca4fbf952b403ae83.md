```
class Plugin { /*...*/ };  // Defines the requirements for plugins

//---- <yourcode/Editor.h> ----------------
#include <yourcode/Plugin.h>

class Editor { /*...*/ };

//---- <thirdparty/VimModePlugin.h> ----------------
#include <yourcode/Plugin.h>  // Correct direction of dependencies

class VimModePlugin : public Plugin { /*...*/ };
```

Again, in order to get a proper dependency inversion, the abstraction must be owned by the high-level. In this context, the Plugin class represents the set of requirements that needs to be fulfilled by all plugins (see again “Guideline 6: Adhere to the Expected Behavior of Abstractions”). The Editor defines and thus owns these requirements. It does not depend on it. Instead, the different plugins depend on the requirements. That is dependency inversion. Hence the DIP is not just about the introduction of an abstraction, but also about the ownership of that abstraction.

# Dependency Inversion via Templates

So far I might have given you the impression that the Dependency Inversion Principle is only concerned with inheritance hierarchies and base classes. However, dependency inversion is also achieved by means of templates. In that context, however, the question of ownership is resolved automatically. As an example, let us consider the std::copy_if() algorithm:

```
template< typename InputIt, typename OutputIt, typename UnaryPredicate >
OutputIt copy_if( InputIt first, InputIt last, OutputIt d_first, UnaryPredicate pred );
```