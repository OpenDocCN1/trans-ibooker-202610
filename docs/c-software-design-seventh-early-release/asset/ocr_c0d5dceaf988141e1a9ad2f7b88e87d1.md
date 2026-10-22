```
bool operator==( Square const& lhs, Square const& rhs )
{
    return lhs.side() == rhs.side();
}
```

In order to make equality-comparison work, you could use a dynamic_cast (10). However, this implementation of equality-comparison holds two severe disadvantages: first, as you have seen in “Guideline 18: Beware the Performance of Acyclic Visitors” a dynamic_cast does most certainly not count as a fast operation. Hence you would have to pay a considerable runtime cost for every comparison. Second, in this implementation you can only successfully compare two Shapes if they are equipped with the same DrawStrategy. While this might be reasonable in some context, it might also be considered an unfortunate limitation in another context. The only solution I am aware of is to return to std::function to store the drawing Strategy, which however would result in another performance penalty.7 In summary, depending on the context, equality comparison may be possible, but it is usually neither easy nor cheap to accomplish. This is evidence to my earlier statement, that Type Erasure does not support binary operations.

# Interface Segregation of Type Erasure Wrappers

“What about the Interface Segregation Principle (ISP)?” you are asking. “While using External Polymorphism, it was easy to separate concerns in the base class. It appears we have lost this ability, right?” Excellent question. So you remember my example with the JSONExportable and Serializable base classes in “Guideline 31: Use External Polymorphism for Non-Intrusive Runtime Polymorphism”. Indeed, with Type Erasure we are no longer able to use the hidden base class anymore, but only the abstracting value type. Therefore it may appear as if the ISP is out of reach:

```
class Document  // Type-erased 'Document'
{
public:
```