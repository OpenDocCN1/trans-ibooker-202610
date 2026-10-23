
```cpp
return *this;
}

//** access x component as element of Fp
ffe_element GetX() const { return xCoordValue_; }

//** access y component as element of Fp
ffe_element GetY() const { return yCoordValue_; }

//** calculate the order of this point by
//** brute-force additions
unsigned int ComputingOrderBruteForceAddition(
    unsigned int maximum_period = ~0) const
{
    EllipticCurvePoint ecPoint = *this;
    unsigned int order = 0;
    while (ecPoint.xCoordValue_ != 0 &&
           ecPoint.yCoordValue_ != 0)
    {
        ++order;
        ecPoint += *this;
        if (order > maximum_period) break;
    }
    return order;
}

//** negation operator (-) that
//** gives the inverse of a point
EllipticCurvePoint operator-()
{
    return EllipticCurvePoint(xCoordValue_, -yCoordValue_);
}

//** equal (==) operator
friend bool operator==(const EllipticCurvePoint& lhsPoint,
                       const EllipticCurvePoint& rhsPoint)
{
    return (lhsPoint.ellipticCurve_ == rhsPoint.ellipticCurve_)
        && (lhsPoint.xCoordValue_ == rhsPoint.xCoordValue_)
        && (lhsPoint.yCoordValue_ == rhsPoint.yCoordValue_);
}

//** different (!=) operator
friend bool operator!=(const EllipticCurvePoint& lhsPoint,
                       const EllipticCurvePoint& rhsPoint)
{
    return (lhsPoint.ellipticCurve_ != rhsPoint.ellipticCurve_)
        || (lhsPoint.xCoordValue_ != rhsPoint.xCoordValue_)
        || (lhsPoint.yCoordValue_ != rhsPoint.yCoordValue_);
}

//** Implementation of a + b operator
friend EllipticCurvePoint operator+(const EllipticCurvePoint& lhsPoint,
                                    const EllipticCurvePoint& rhsPoint)
{
    ffe_element xResult, yResult;
    lhsPoint.ETwoPointsAddition(
        lhsPoint.xCoordValue_,
        lhsPoint.yCoordValue_,
        rhsPoint.xCoordValue_,
        rhsPoint.yCoordValue_,
        xResult, yResult);
    return EllipticCurvePoint(xResult, yResult, *lhsPoint.ellipticCurve_);
}

//** multiplying with scalar * int
friend EllipticCurvePoint operator*(int scalar,
                                    const EllipticCurvePoint& rhsPoint)
{
    return EllipticCurvePoint(rhsPoint).operator*=(scalar);
}

//** Implementation of += operator
EllipticCurvePoint& operator+=(const EllipticCurvePoint& rhsPoint)
{
    ECTwoPointsAddition(xCoordValue_, yCoordValue_,
                        rhsPoint.xCoordValue_, rhsPoint.yCoordValue_,
                        xCoordValue_, yCoordValue_);
    return *this;
}

//** Implementation of *= int operator
EllipticCurvePoint& operator*=(int scalar)
{
    return (*this = MultiplyUsingScalar(scalar, *this));
}

//** display and print the point
//** using ostream
friend ostream& operator<<(ostream& os, const EllipticCurvePoint& p)
{
    return (os << "(" << p.xCoordValue_ << ", " << p.yCoordValue_ << ")");
}
};
```

## 第9章 椭圆曲线密码学

```cpp
typedef EllipticCurve<OrderFFE_EC> this_t;
typedef class EllipticCurve<OrderFFE_EC>::EllipticCurvePoint point_t;

//** the Weierstrass equation
//** as y^2 = x^3 + ax + b
EllipticCurve(int CoefA, int CoefB)
    : ECParameterA(CoefA),
      ECParameterB(CoefB),
      tableOfPoints(),
      tableFilledComputed(false)
{
}

//** compute all the points
//** (from the group of elements) for
//** Weierstrass equation. Note the
//** fact that if we are
//** having a high order for the curve,
//** the computation process
//** will take some time
void CalculatePoints()
{
    int x_val[OrderFFE_EC];
    int y_val[OrderFFE_EC];
    for (int counter = 0; counter < OrderFFE_EC; ++counter)
    {
        int nsq = counter * counter;
        x_val[counter] = ((counter * nsq) +
                          ECParameterA.i() * counter +
                          ECParameterB.i()) % OrderFFE_EC;
        y_val[counter] = nsq % OrderFFE_EC;
    }

    for (int counter1 = 0; counter1 < OrderFFE_EC; ++counter1)
    {
        for (int counter2 = 0; counter2 < OrderFFE_EC; ++counter2)
        {
            if (x_val[counter1] == y_val[counter2])
            {
                tableOfPoints.push_back(
                    EllipticCurvePoint(counter1, counter2, *this));
            }
        }
    }
    tableFilledComputed = true;
}

//** obtain the point (from the group of
//** elements) for the curve
EllipticCurvePoint operator[](int n)
{
    if (!tableFilledComputed)
    {
        CalculatePoints();
    }

    return tableOfPoints[n];
}

//** the number of the elements
//** in the group
size_t Size() const { return tableOfPoints.size(); }

//** the degree of the point for
//** the elliptic curve
int Degree() const { return OrderFFE_EC; }

//** the "a" parameter, as an element of Fp
FFE<OrderFFE_EC> a() const { return ECParameterA; }

//** the "b" parameter, as an element of Fp
FFE<OrderFFE_EC> b() const { return ECParameterB; }

//** print and show the elliptic curve in a
//** readable format using ostream human
//** readable form
template<int ECT>
friend ostream& operator<<(ostream& os,
                           const EllipticCurve<ECT>& EllipticCurve);

//** print and display all the elements
//** of the elliptic curve group
ostream& PrintTable(ostream& os, int columns = 4);

private:
    typedef std::vector<EllipticCurvePoint> TableWithPoints;

    //** table with the points
    TableWithPoints tableOfPoints;

    //** first parameter of the
    //** elliptic curve equation
    FFE<OrderFFE_EC> ECParameterA;

    //** second parameter of the
    //** elliptic curve equation
    FFE<OrderFFE_EC> ECParameterB;

    //** boolean value to show if the
    //** table has been computed
    bool tableFilledComputed;
};

template<int ECT>
typename EllipticCurve<ECT>::EllipticCurvePoint
EllipticCurve<ECT>::EllipticCurvePoint::ONE(0, 0);

template<int ECT>
ostream& operator<<(ostream& os,
                    const EllipticCurve<ECT>& EllipticCurve)
{
    os << "y^2 mod " << ECT << " = (x^3" << showpos;
    if (EllipticCurve.ECParameterA != 0)
    {
        os << EllipticCurve.ECParameterA << "x";
    }

    if (EllipticCurve.ECParameterB.i() != 0)
    {
        os << EllipticCurve.ECParameterB;
    }

    os << noshowpos << ") mod " << ECT;
    return os;
}

template<int P>
ostream& EllipticCurve<P>::PrintTable(ostream& os, int columns)
{
    if (tableFilledComputed)
    {
        int col = 0;
        typename EllipticCurve<P>::TableWithPoints::iterator iter =
            tableOfPoints.begin();
        for (; iter != tableOfPoints.end(); ++iter)
        {
            os << "(" << (*iter).xCoordValue_.i() <<
                  ", " << (*iter).yCoordValue_.i() << ") ";
            if (++col > columns)
            {
                os << "\n";
                col = 0;
            }
        }
    }
    else
    {
        os << "EllipticCurve, F_" << P;
    }
    return os;
}
}

namespace utils
{
    float frand()
    {
        static float norm = 1.0f / (float)RAND_MAX;
        return (float)rand() * norm;
    }

    int irand(int min, int max)
    {
        return min + (int)(frand() * (float)(max - min));
    }
}

using namespace EllipticCurveCryptography;
using namespace utils;

int main(int argc, char *argv[])
{
    typedef EllipticCurve<163> elliptic_curve;
    elliptic_curve myEllipticCurve(1, 1);

    cout << "Basic Example of using Elliptic Curve Cryptography using C++20. Apress, 2020\n\n";

    //** display some information about the
    //** elliptic curve and display some of the properties
    cout << "Equation of the elliptic curve: " << myEllipticCurve << "\n";

    //** compute the points for the elliptic
    //** curve for equation from the above
    myEllipticCurve.CalculatePoints();

    cout << "\nList of the points (x,Y) for the curve (i.e. the group elements):\n";
    myEllipticCurve.PrintTable(cout, 5);
    cout << "\n\n";

    elliptic_curve::EllipticCurvePoint P = myEllipticCurve[2];
    cout << "Randomly - Point P = " << P << ", 2P = " << (P + P) << "\n";

    elliptic_curve::EllipticCurvePoint Q = myEllipticCurve[3];
    cout << "Randomly - Point Q = " << Q << ", P+Q = " << (P + Q) << "\n";

    elliptic_curve::EllipticCurvePoint R = P;
    R += Q;
    cout << "P += Q = " << R << "\n";
```
