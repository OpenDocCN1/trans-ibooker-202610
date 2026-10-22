behavior and provide an abstraction for it in the form of a concept (see “Guideline 7: Understand the Similarities between Base Classes and Concepts”). This, in contrast to the OO form of Strategy, does not incur any runtime performance penalty.

A similar approach can be seen in the std::unique_ptr class template:

```
namespace std {

template< typename T, typename Deleter = std::default_delete<T> >
⑧
class unique_ptr;

template< typename T, typename Deleter > ⑩
class unique_ptr<T[], Deleter>;

} // namespace std
```

For both the base template (⑧), and its specialization for arrays (⑩), it is possible to specify an explicit Deleter as the second template argument. With this argument you can decide whether you want to free the resource by means of delete, by means of free() or any other deallocation function. It is even possible to “abuse” std::unique_ptr to perform a completely different kind of cleanup.

This flexibility is also evidence for the Strategy design pattern. The template argument allows you to inject some cleanup behavior into the class. This form of Strategy is also called Policy-Based Design, based on a design philosophy introduced by Andrei Alexandrescu in 2001.⁹ The idea is the same: extract and isolate specific behavior of class templates in order to improve changeability, extensibility, testability, and reusability. Thus Policy-Based Design can be considered the static polymorphism form of the Strategy design pattern. And evidently, the design works really well, as the many applications of this idea in the Standard Library demonstrate.

You can also apply Policy-Based Design to the drawing of shapes example. Consider the following implementation of the Circle class: