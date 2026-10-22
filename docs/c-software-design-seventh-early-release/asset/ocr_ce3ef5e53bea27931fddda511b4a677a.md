At this point, you are looking very suspiciously, almost accusingly, in my direction. Didn’t I say that having both would not be possible? Obviously it is possible, right? Well, once again, I did not claim that it is impossible. I rather said that this might be impractical. Now that you have seen the advantage of an *Acyclic Visitor*, let me show you the downsides of this approach.

First, let us take a look at the implementation of the `accept()` function in Circle:

```
//---- <Circle.h> ----------------

class Circle : public Shape
{
public:
    explicit Circle( double radius )
        : radius_( radius )
    {
        /* Checking that the given radius is valid */
    }

    void accept( AbstractVisitor const& v ) override { ③
        if( auto const* cv = dynamic_cast<Visitor<Circle> const*> (&v) ) { ④
            cv->visit( *this ); ⑤
        }
    }

    double radius() const { return radius_; }
    Point center() const { return center_; }

private:
    double radius_;
    Point center_{};
};
```

You might have noticed the one small change in the Shape hierarchy, which is that the virtual `accept()` function now accepts an AbstractVisitor ③. You also remember that the AbstractVisitor does not implement any operation on its own. Therefore, instead of calling a `visit()` function on the