```
}

void draw( /*some arguments*/ ) const override
{
    drawer_( *this, /*some arguments*/ );
}

double radius() const { return radius_; }

private:
    double radius_;
    DrawStrategy drawer_;  ②
};

//---- <Square.h> ----------------

#include <Shape.h>
#include <memory>
#include <functional>
#include <utility>

class Square : public Shape
{
public:
    using DrawStrategy = std::function<void(Square const&, /*...*/ )>;  ⑨

    explicit Square( double side, DrawStrategy drawer )  ⑩
        : side_( side )
        , drawer_( std::move(drawer) )  ⑪
    {
        /* Checking that the given side length is valid and that
           the given 'std::function' instance is not empty */
    }

    void draw( /*some arguments*/ ) const override
    {
        drawer_( *this, /*some arguments*/ );
    }

    double side() const { return side_; }

private:
    double side_;
    DrawStrategy drawer_;  ⑫
};
```