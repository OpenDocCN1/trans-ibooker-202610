```
class Shape
{
public:
Shape() = default;
virtual ~Shape() = default;
virtual void draw() const = 0;
// ...
};
```

```
template< typename T >
class DrawStrategy
{
public:
virtual ~DrawStrategy() = default;
virtual void draw( T const& ) const = 0;
};
```

High-level

Architectural Boundary

```
class Circle : public Shape
{
public:
Circle( double rad
, std::unique_ptr<DrawCircleStrategy> strategy );

void draw() const override;
// ...
};
```

Architectural Boundary

Inversion of dependencies

Low-level

```
class OpenGLCircleStrategy : public DrawStrategy<Circle>
{
public:
void draw( Circle const& circle ) const override;
};
```