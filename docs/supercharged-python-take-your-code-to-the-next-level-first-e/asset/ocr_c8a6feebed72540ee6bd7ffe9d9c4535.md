| ) |
|---|
| **end(n)** Similar to **start(n)**, except that **end(n)** gets the ending position of the identified group, relative to the entire target string. Within this string, the text consists of all characters within the target string, beginning with the “start” index, up to but not including the “end” index. For example, start and end values of 0 and 3 means that the first three characters were matched. |
| **span(n)** Returns the information provided by **start(n)** and **end(n)** but returns it in tuple form. |
| **lastindex** The highest index number among the groups. |

# # 6.9 SEARCHING A STRING FOR PATTERNS

Once you understand the basic regular-expression syntax, you can apply it in many useful ways. So far, we’ve used it to look for