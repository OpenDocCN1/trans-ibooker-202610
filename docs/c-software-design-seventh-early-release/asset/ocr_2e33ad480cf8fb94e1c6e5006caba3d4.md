```
(/*...red...*/ ) );
    shapes.emplace_back(
        std::make_unique<Square>(
            1.2, std::make_unique<OpenGLSquareStrategy>
        (/*...green...*/ ) );
    shapes.emplace_back(
        std::make_unique<Circle>(
            4.1, std::make_unique<OpenGLCircleStrategy>
        (/*...blue...*/ ) );

    // Drawing all shapes
    for( auto const& shape : shapes )
    {
        shape->draw( /*some arguments*/ );
    }

    return EXIT_SUCCESS;
}
```

# Comparison between Visitor and Strategy

As you have now heard about both the *Visitor* and *Strategy* design patterns, you might wonder what the difference between the two is. After all, the implementation looks fairly similar. But while there are parallels in the implementation, the properties of the two design patterns are very different. In case of the *Visitor* design pattern we have identified the *general* addition of operations as the *variation point*. Therefore we have created an abstraction for operations in general, which in turn allowed everyone to add operations. The unfortunate side effect was, that it was no longer easy to add new shape types.

In case of the *Strategy* design pattern we have identified the implementation details of a *single* function as a *variation point*. After introducing an abstraction for these implementation details we are still able to easily add new types of shapes, but we are not able to easily add new operations. That is, because adding an operation would still require you to intrusively add a virtual member function. Hence the intent of the *Strategy* design pattern is the opposite of the intent of the *Visitor* design pattern.

It may sound promising to combine the two design patterns in order to gain the advantages of both ideas (making it easy to add both types and