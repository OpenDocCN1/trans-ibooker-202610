```
//---- <Circle.h> ----------------

#include <Shape.h>
#include <DrawCircleStrategy.h>
#include <SerializeCircleStrategy.h>
#include <memory>
#include <utility>

class Circle : public Shape
{
public:
    explicit Circle( double radius
                    , std::unique_ptr<DrawCircleStrategy> drawer
                    , std::unique_ptr<SerializeCircleStrategy> serializer
                    /* potentially more strategy-related arguments */ )
        : radius_( radius )
        , drawer_( std::move(drawer) )
        , serializer_( std::move(serializer) )
        // ...
    {
        /* Checking that the given radius is valid and that
           the given std::unique_ptrs are not nullptrs */
    }

    void draw( /*some arguments*/ ) const override
    {
        drawer_->draw( *this, /*some arguments*/ );
    }

    void serialize( /*some arguments*/ ) const override
    {
        serializer_->serialize( *this, /*some arguments*/ );
    }

    double radius() const { return radius_; }

private:
    double radius_;
    std::unique_ptr<DrawCircleStrategy> drawer_;
    std::unique_ptr<SerializeCircleStrategy> serializer_;
    // ... Potentially more strategy-related data members
};
```