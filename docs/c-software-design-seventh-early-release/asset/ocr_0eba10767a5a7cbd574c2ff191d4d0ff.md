```
either. ③
    return EXIT_SUCCESS;
}
```

Although both Meter and Kilometer are represented via long, it is not be possible to directly add Meter and Kilometer together ①. And this is great: it does not leave any opening for accidental bugs to crawl in. It is also not possible to add two Surnames, although a std::string itself provides an addition operator for the purpose of string concatenation ②. But this is also great: the strong type effectively restricts undesired operations of the underlying type. Unfortunately, this “feature” also prevents the addition of two Meter instances ③. This operation would be desirable, though: it is intuitive, natural, and since the result of the operation would again be of type Meter, it would be physically accurate. In order to make this work, we could implement an addition operator for the Meter type. However, obviously, this would not remain the only addition operator. We would also need one for all the other strong types, such as Kilometer, Mile, Foot, etc. Since all of these implementations would look the same, this would be a violation of the Don’t Repeat Yourself (DRY) principle. Therefore it appears to be reasonable to extend the StrongType class template with an addition operator:

```
template< typename T, typename Tag >
StrongType<T,Tag>
operator+( StrongType<T,Tag> const& a, StrongType<T,Tag> const& b )
{
    return StrongType<T,Tag>( a.get() + b.get() );
}
```

Whereas due to the formulation of this addition operator it is not possible to add two different instantiations of StrongType together (e.g. Meter and Kilometer), it would enable the addition of two instances of the same instantiation of StrongType. “Oh, but I see a problem: while it would now be possible to add two Meters or two Kilometers, it would also be possible to add two Surnames. We don’t want that!” You are correct: this