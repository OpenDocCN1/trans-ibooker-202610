

## Listing 14-5. 主程序

```cpp
#include "encryption.h"
#include "generation.h"

#include <fcntl.h>
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <windows.h>
#include <time.h>
#include <inttypes.h>
#include <iostream>

using namespace std;

const size_t MESSAGE_LENGTH = 2000000000;

uint64_t generateStringOfBytes()
{
    int folder = 0;
    ssize_t resourceFile = 0;
    uint64_t buffer = 0;

    if((folder = open("D:/Apps C++/Chapter 14 - Chaos-based Cryptography/ChaosSecureRandomNumberGenerator/dev/urandom",
    O_RDONLY)) == -1)
        exit(-1);

    if((resourceFile = read(folder, &buffer, sizeof buffer)) < 0)
        exit(-1);

    buffer &= ((1ULL << 53) - 1);
    close(folder);
    return buffer;
}

int main(void)
{
        struct generation gen;
        uint64_t key[3] = {generateStringOfBytes()+rand()%3000, generateStringOfBytes()+rand()%5000, generateStringOfBytes()+rand()%8000};
        cout<<"Key 1 -> "<<key[0]<<endl;
        cout<<"Key 2 -> "<<key[1]<<endl;
        cout<<"Key 3 -> "<<key[2]<<endl;

        // generate 1GiB of 1s
        uint8_t *message = (uint8_t*)malloc(MESSAGE_LENGTH);
        memset(message, 1, MESSAGE_LENGTH);

        // perform encryption
        generation_initialization(&gen, key);
        clock_t s = clock();
        encryption(&gen, message, MESSAGE_LENGTH);
        clock_t e = clock();
        double spent = (double)(e - s) / CLOCKS_PER_SEC;
        printf("1GiB in %lfs\n", spent);

        free(message);
}
```

## Cipher Using Chaos and Fractals

在本节中，我们将讨论并实现一种使用混沌和分形概念的加密/解密操作的解决方案。

Listing 14-6 包含了处理起始点表示的主要函数声明，并对 x 和 y 两个轴执行投影。需要提到的是，使用分形和混沌系统时最具挑战性的操作和任务之一是识别路径并识别主根（参见函数 identifyFirstRoot()）。Listing 14-6 和 14-7 中的代码相当自解释，并且包含必要的注释以便完全理解。参见图 14-5 查看应用程序的执行。

```cpp
// get the sign of a double number
inline double getSign(double number)
{
    // in case that d is less than 0, return -1.0, making the number
    // negative
    // contrary make the number positive
    if (number<0)
        return(-1.0);
    else
        return(1.0);
};

// Value 'yValue' will be projected over an integer matrix or grid.
// We have choose this for achieving the scaling goal and performing
//     tests.
// The projection process is a matter of personal choice, any other
// idea or
// solution can be implemented by reader.
inline unsigned int PerformProjectionFor_Y(double yValue)
{
    unsigned long q;
    const double scale=(32768.0/2.0);
    const double offset=(32768.0);

    // do the projection as a positive integer projection to positive
    //     integer
    q=(yValue*scale)+offset;

    // getting the LSB (least significant bit)
    q&=1;
    return q;
}

// Value 'xValue' will be projected over an integer matrix or grid.
// We have choose this for achieving the scaling goal and performing
//     tests.
// The projection process is a matter of personal choice, any other
// idea or
// solution can be implemented by reader.
inline unsigned int PerformProjectionFor_X(double xValue)
{
    // used for storing the decomposition value
    double decompositionValue;

    // power value (exponent)
    int n;

    // with frexp() we will decompose the double point (xValue) as
    // argument into a normalized fraction and an integral power
    decompositionValue = frexp (xValue , &n);

    // with ldexp() we will return the result of multiplying
    // 'decompositionValue'
    // (the significand) with 2 and raised to the power '51'
    // (exponent)
    decompositionValue = ldexp(decompositionValue,51);

    // Test if the difference between 'decompositionValue' and
    // floor(decompositionValue) is less than 0.5
    // if yes return '1', otherwise '0'.
    // With floor() we round 'decompositionValue', returning the
    // largest
    // integral value that is not greater than 'decompositionValue'
    return (((decompositionValue-floor(decompositionValue))<0.5)?1:0);
}

inline void identifyFirstRoot()
{
    /* Zn*Zn=Z(n+1)-c */
    z_xCoordinatePoint=z_xCoordinatePoint-c_xCoordinatePoint;
    z_yCoordinatePoint=z_yCoordinatePoint-c_yCoordinatePoint;
}

// r represents the length of the vector from the origin to the point
// r = |z| = sqrt(x*x+y*y)
double r;

// the new point z = (x,y)
double z_xNewPointValue, z_yNewPointValue;      //NewZx, NewZy
r=sqrt(z_xCoordinatePoint*z_xCoordinatePoint+z_yCoordinatePoint*z_yCoordinatePoint);

// the below code sequence represents the implementation of the algorithm presented in [17], from page 361 to 362.
// case 1: z>0
if (z_xCoordinatePoint>0)
{
    z_xNewPointValue=sqrt(0.5*(z_xCoordinatePoint+r));
    z_yNewPointValue=z_yCoordinatePoint/(2*z_xNewPointValue);
}

// for cases when z<0 and z=0
else
{
    // case 2: z<0
    if (z_xCoordinatePoint<0)
    {
        z_yNewPointValue=getSign(z_yCoordinatePoint)*sqrt(0.5*(-z_xCoordinatePoint+r));
        z_xNewPointValue=z_yCoordinatePoint/(2*z_yNewPointValue);
    }

    // case 3: z=0
    else
    {
        z_xNewPointValue=sqrt(0.5*fabs(z_yCoordinatePoint));
        if (z_xNewPointValue>0) z_yNewPointValue=z_yCoordinatePoint/(2*z_xNewPointValue);
        else z_yNewPointValue=0;
    }
}

};
// end of the implementation

// the values for x and y coordinates
z_xCoordinatePoint=z_xNewPointValue;
z_yCoordinatePoint=z_yNewPointValue;
};
public:
    // gets the encrypted value
    unsigned int getEncryptedMessageA(unsigned int plainValue);
    unsigned int getDecryptedMessageB(unsigned int encryptedValue);
    unsigned int getEncryptedMessageC(unsigned int stream);
    unsigned int getDecryptedMessageD(unsigned int stream);

    // gets the single bit
    unsigned int bitCodeEncryptedMessageA(unsigned int plainValue);
    unsigned int bitCodeDecryptedMessageB(unsigned int encryptedValue);
    unsigned int bitCodeEncryptedMessageC(unsigned int stream);
    unsigned int bitCodeDecryptedMessageD(unsigned int stream);

    // constructor
    CryptoFractalCipher(double cx,double cy);

    // destructor
    virtual ~CryptoFractalCipher();
};
#endif
```

## Listing 14-7. 主程序

```cpp
#include "FractalCipherCrypto.h"
#include <climits>
#include <assert.h>
#include <math.h>
#include <iostream>

using namespace std;

// implementing bitCodeEncryptedMessageA from FractalCipherCrypto.h file
unsigned int CryptoFractalCipher::bitCodeEncryptedMessageA(unsigned int bit_from_plaintext)
{
    // below we will create a cryptographic stream from the clear stream
    int crypto_bit=0;
    {
        identifyFirstRoot();

        // quadratic value
        unsigned long quadraticValue = PerformProjectionFor_X(
        z_yCoordinatePoint);

        // Do the encoding process and provide the
        // cryptographic stream from the clear stream
        // Variables used:
        //          - iV: the input value
        //          - oV: the output value
        //          - rV: the route value in the expansion of the fractal
        unsigned int iV, oV, rV;
        {
            unsigned int result1, result2, result3;
            iV=(bit_from_plaintext) & 1;

            // obtained from the iteration of the quadratic value
            result1=quadraticValue;

            // input value
            result2=iV;

            // we will copy the bits if it is set in one operand but not both
            result3=result1^result2;

            // the final output value
            oV=result3;

            // the route value that need to be followed within the expansion of the fractal
```
