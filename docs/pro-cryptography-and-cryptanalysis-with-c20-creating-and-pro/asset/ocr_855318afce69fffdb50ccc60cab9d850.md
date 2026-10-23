```
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

    // do the projection as a positive integerproject to positive
    //     integer
    q=(yValue*scale)+offset;

    //getting the LSB (least significant bit)
    q&=1;
    return q;
}

// Value 'xValue' will be projected over an integer matrix or grid.
// We have choose this for achieving the scaling goal and performing
//     tests.
```
```