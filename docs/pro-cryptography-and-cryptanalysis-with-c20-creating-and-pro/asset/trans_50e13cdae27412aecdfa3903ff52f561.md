

namespace HelperFunctionFFE
{
    //** Computing Extended GCD gives g = a*u + b*v
    int ExtendedGreatestCommonDivisor(int a, int b,
                                    int& u, int &v)
    {
        u = 1;
        v = 0;
        int g = a;
        int u1 = 0;
        int v1 = 1;
        int g1 = b;
        while (g1 != 0)
        {
            //** division using integers
            int q = g/g1;
            int t1 = u - q*u1;
            int t2 = v - q*v1;
            int t3 = g - q*g1;
            u = u1; v = v1; g = g1;
            u1 = t1; v1 = t2; g1 = t3;
        }
        return g;
    }

    //** providing solution and solving
    //** the linear congruence equation
    //** x * z == 1 (mod n) for z
    int InvMod(int x, int n)
    {
        //** "%" represents the remainder
        //** function, 0 <= x % n < |n|
        x = x % n;
        int u,v,g,z;
        g = ExtendedGreatestCommonDivisor(x, n,u,v);

if (g != 1)
{
    //** x and n has to be primes
    //** in order to exist an x^-1 mod n
    z = 0;
}
else
    z = u % n;

return z;
}
}

//** represents the element from a Galois field
//** we will use a specific behaviour for the
//** modular function in which (-n) mod m will
//** return a negative number.
//** The implementation is done in such way that
//** it will offer a support for the basic
//** arithmetic operations, such as:
//** + (addition), - (subtraction), / (division)
//** and scalar multiplication.
//** The P served as an argument represents the
//** order for the field.
template<int P>
class FFE
{
int i_;

void assign(int i)
{
    i_ = i;
    if ( i<0 )
    {
        //** The correction behaviour
        //** is important.
        //** Using (-i) mod p we will make sure

//** implementation of "!=" operator
friend bool operator!=(const FFE<P>& lhs, int rhs)
{
    return (lhs.i_ != rhs);
}

// implementation of "a/b" operator
friend FFE<P> operator/(const FFE<P>& lhs,
                    const FFE<P>& rhs)
{
    return FFE<P>( lhs.i_ *
        HelperFunctionFFE::InvMod(rhs.i_,P));
}

//** implementation of "a+b" operator
friend FFE<P> operator+(const FFE<P>& lhs,
                    const FFE<P>& rhs)
{
    return FFE<P>( lhs.i_ + rhs.i_);
}

//** implementation of "a-b" operator
friend FFE<P> operator-(const FFE<P>& lhs,
                    const FFE<P>& rhs)
{
    return FFE<P>(lhs.i_ - rhs.i_);
}

// implementation of "a + int" operator
friend FFE<P> operator+(const FFE<P>& lhs, int i)
{
    return FFE<P>( lhs.i_+i);
}

//** implementation of "int + a" operator
friend FFE<P> operator+(int i, const FFE<P>& rhs)
{
    return FFE<P>( rhs.i_+i);
}

//** implementation of "int * a" operator
friend FFE<P> operator*(int n, const FFE<P>& rhs)
{
    return FFE<P>( n*rhs.i_);
}

//** implementation of "a * b"
friend FFE<P> operator*(const FFE<P>& lhs,
                    const FFE<P>& rhs)
{
    return FFE<P>( lhs.i_ * rhs.i_);
}

//** the operator ostream for
//** showing and displaying in
//** readable format
template<int T>
friend ostream& operator<<(ostream& os,
                    const FFE<T>& g)
{
    return os << g.i_;
}
};
}

## Listing 9-2. Implementation of the Main Program

```
//** Leave everything as it is.
//** Don't change the order of the inputs or namespaces.

#include <cstdlib>
#include <iostream>
#include <vector>

using namespace std;

#include <math.h>
#include "FFE_Engine.hpp"

namespace EllipticCurveCryptography
{
    //** Elliptic Curve over a finite field of order P:
    //** y^2 mod P = x^3 + ax + b mod P
    template<int OrderFFE_EC> class EllipticCurve
    {
        public:
            //** this curve is defined over the finite
            //** field (Galois field) Fp, this is the
            //** typedef of elements in it
            typedef FFE<OrderFFE_EC> ffe_element;

            //** any point on elliptic curve is formed
            //** from two elements that are within Fp
            //**field (Galois Field). The points are
            //** created once we declare an instance of
            //** Elliptic Curve itself.
            class EllipticCurvePoint
            {
                friend class EllipticCurve<OrderFFE_EC>;
                typedef FFE<OrderFFE_EC> ffe_element;
                ffe_element xCoordValue_;
                ffe_element yCoordValue_;
                EllipticCurve *ellipticCurve_;
```

## CHAPTER 9    ELLIPTIC-CURVE CRYPTOGRAPHY

```
/** core of the doubling multiplier
/** algorithm (see below)
/** multiplies acc by m as a series of
/** "2*accumulatorContainer's"
void DoublingMultiplierAlgorithm(int
                         multiplier, EllipticCurvePoint&
                         accumulatorContainer)
{
    if (multiplier > 0)
    {
        EllipticCurvePoint doublingValue =
                         accumulatorContainer;
        for (int counter=0; counter <
                         multiplier; ++counter)
        {
            /** doubling step
            doublingValue += doublingValue;
        }
        accumulatorContainer =
                         doublingValue;
    }
}

/** Implementation of doubling
/** multiplier algorithm.
/** The process stands on multiplying
/** intermediateResultAccumulator for
/** storing the intermediate
/** results with inputScalar.
/** This is done through
/** expansion in multiple
/** by 2 between the first of the
/** binary representation of inputScalar.
```

EllipticCurvePoint MultiplyUsingScalar(int
    inputScalar, const EllipticCurvePoint&
    intermediateResultAccumulator)
{
    EllipticCurvePoint
        accumulatorContainer =
        intermediateResultAccumulator;
    EllipticCurvePoint outputResult =
        EllipticCurvePoint(0,0,
        *ellipticCurve_);

    int i = 0, j = 0;
    int iS = inputScalar;

    while(iS)
    {
        if (iS&1)
        {
//** Setting up the bit.
//** The computation is done by following the formula:
//** accumulatorContainer = 2^(i-j)*accumulatorContainer
DoublingMultiplierAlgorithm(i-j,accumulatorContainer);

outputResult += accumulatorContainer;

//** last setting for the bit
j = i;
        }
        iS >>= 1;
        ++i;
    }
    return outputResult;
}

//** the function deals with
//** adding two points on the curve

if (xCoord1 == xCoord2 && yCoord1 == yCoord2)
{
    //** computing 2*P
    s = (3*(xCoord1.i()*xCoord1.i()) + ellipticCurve_->a()) / (2*yCoord1);
    xCoordR = ((s*s) - 2*xCoord1);
}
else
{
    //** computing P+Q
    s = (yCoord1 - yCoord2) / (xCoord1 - xCoord2);
    xCoordR = ((s*s) - xCoord1 - xCoord2);
}

if (s!=0)
{
    yCoordR = (-yCoord1 + s*(xCoord1 - xCoordR));
}
else
{
    xCoordR = yCoordR = 0;
}

EllipticCurvePoint(int xPoint, int yPoint)
: xCoordValue_(xPoint),
  yCoordValue_(yPoint),
  ellipticCurve_(0)
{}

## CHAPTER 9   ELLIPTIC-CURVE CRYPTOGRAPHY

EllipticCurvePoint(int xPoint, int yPoint,
                    EllipticCurve<OrderFFE_EC> &
                    EllipticCurve)
    : xCoordValue_(xPoint),
      yCoordValue_(yPoint),
      ellipticCurve_(&EllipticCurve)
    {}

EllipticCurvePoint(const ffe_element&
                   xPoint, const ffe_element& yPoint,
                   EllipticCurve<OrderFFE_EC> &
                   EllipticCurve)
    : xCoordValue_(xPoint),
      yCoordValue_(yPoint),
      ellipticCurve_(&EllipticCurve)
    {}

public:
    static EllipticCurvePoint ONE;

    /** constructor
    EllipticCurvePoint(const
                       EllipticCurvePoint& rhsPoint)
    {
        xCoordValue_ = rhsPoint.xCoordValue_;
        yCoordValue_ = rhsPoint.yCoordValue_;
        ellipticCurve_ =
            rhsPoint.ellipticCurve_;
    }

    /** the assignment process
    EllipticCurvePoint& operator=(const
                       EllipticCurvePoint& rhsPoint)
    {
        xCoordValue_ = rhsPoint.xCoordValue_;
        yCoordValue_ = rhsPoint.yCoordValue_;
        ellipticCurve_ =
            rhsPoint.ellipticCurve_;
    }

