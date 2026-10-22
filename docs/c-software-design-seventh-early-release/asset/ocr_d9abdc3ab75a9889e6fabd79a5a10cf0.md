reference semantics. So the question is: how can we leverage the semantic advantages, but stick to value semantics? The solution comes in the form of std::optional:31

```
std::optional<int> to_int( std::string_view );
```

std::optional is a value type, which represents any other value, in our example an int. Therefore std::optional can take all the values that an int can take. The speciality of std::optional, however, is that it adds one more state to the wrapped value, a state that represents no value. Thus our std::optional is an int that may or may not be present:

```
#include <charconv>
#include <cstdlib>
#include <optional>
#include <sstream>
#include <string>
#include <string_view>

std::optional<int> to_int( std::string_view sv )
{
    std::optional<int> oi{};
    int i{};

    auto const result = std::from_chars( sv.data(), sv.data() + sv.size(), i );
    if( result.ec != std::errc::invalid_argument ) {
        oi = i;
    }

    return oi;
}

int main()
{
    std::string value = "42";

    if( auto optional_int = to_int( value ) )
    {
        // ... Success: the returned std::optional contains an integer value
    }
    else
```