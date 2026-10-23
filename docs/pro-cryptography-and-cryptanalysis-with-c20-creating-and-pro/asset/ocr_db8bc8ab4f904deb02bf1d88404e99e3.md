```
```cpp
return *this;
}

/** access x component as element of Fp
ffe_element GetX() const { return
                                      xCoordValue_; }

/** access y component as element of Fp
ffe_element GetY() const { return
                                      yCoordValue_; }

/** calculate the order of this point by
/** brute-force additions
unsigned int
             ComputingOrderBruteForceAddition
             (unsigned int maximum_period = ~0) const
{
    EllipticCurvePoint ecPoint = *this;
    unsigned int order = 0;
    while(ecPoint.xCoordValue_ != 0 &&
          ecPoint.yCoordValue_ != 0)
    {
        ++order;
        ecPoint += *this;
        if (order > maximum_period) break;
    }
    return order;
}

/** negation operator (-) that
/** gives the inverse of a point
EllipticCurvePoint operator-()
{
    return
        EllipticCurvePoint(xCoordValue_,
                           -yCoordValue_);
}
```
```