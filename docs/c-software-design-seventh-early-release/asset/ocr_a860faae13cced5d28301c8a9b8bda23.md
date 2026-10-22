```
double radius_;
std::unique_ptr<DrawCircleStrategy> drawer_;
};

//---- <DrawSquareStrategy.h> ----------------

class Square;

class DrawSquareStrategy
{
public:
    virtual ~DrawSquareStrategy() = default;

    virtual void draw( Square const& square, /*some arguments*/ )
const = 0;
};

//---- <Square.h> ----------------

#include <Shape.h>
#include <DrawSquareStrategy.h>
#include <memory>
#include <utility>

class Square : public Shape
{
public:
    explicit Square( double side,
std::unique_ptr<DrawSquareStrategy> drawer )
        : side_( side )
        , drawer_( std::move(drawer) )
    {
        /* Checking that the given side length is valid and that
           the given 'std::unique_ptr' is not a nullptr */
    }

    void draw( /*some arguments*/ ) const override
    {
        drawer_->draw( *this, /*some arguments*/ );
    }

    double side() const { return side_; }

private:
    double side_;
```