```
//** multiplying with scalar * int
friend EllipticCurvePoint operator*(int
    scalar, const
    EllipticCurvePoint& rhsPoint)
{
    return
        EllipticCurvePoint(rhsPoint).
        operator*=(scalar);
}

//** Implementation of += operator
EllipticCurvePoint& operator+=(const
    EllipticCurvePoint& rhsPoint)
{
    ECTwoPointsAddition(xCoordValue_,
    yCoordValue_,rhsPoint.xCoordValue_,
    rhsPoint.yCoordValue_,xCoordValue_,
    yCoordValue_);
    return *this;
}

//** Implementation of *= int operator
EllipticCurvePoint& operator*=(int scalar)
{
    return (*this =
        MultiplyUsingScalar(scalar,*this));
}

//** display and print the point
//** using ostream
friend ostream& operator <<(ostream& os,
    const EllipticCurvePoint& p)
{
    return (os << "(" << p.xCoordValue_ <<
        ", " << p.yCoordValue_ << ")");
}
};
```