```
//** equal (==) operator
friend bool operator==(const
    EllipticCurvePoint& lhsPoint,
    const EllipticCurvePoint& rhsPoint)
{
    return (lhsPoint.ec_ == rhsPoint.ec_)
        && (lhsPoint.x_ == rhsPoint.x_) &&
        (lhsPoint.y_ == rhsPoint.y_);
}

//** different (!=) operator
friend bool operator!=(const
    EllipticCurvePoint& lhsPoint, const
    EllipticCurvePoint& rhsPoint)
{
    return (lhsPoint.ec_ != rhsPoint.ec_)
        || (lhsPoint.x_ != rhsPoint.x_) ||
        (lhsPoint.y_ != rhsPoint.y_);
}

//** Implementation of a + b operator
friend EllipticCurvePoint operator+(const
    EllipticCurvePoint& lhsPoint,
    const EllipticCurvePoint& rhsPoint)
{
    ffe_element xResult, yResult;
    lhsPoint.ETwoPointsAddition(
        lhsPoint.xCoordValue_,
        lhsPoint.yCoordValue_,
        rhsPoint.xCoordValue_,
        rhsPoint.yCoordValue_,
        xResult,yResult);
    return
        EllipticCurvePoint(xResult,
            yResult,
            *lhsPoint.ellipticCurve_);
}
```