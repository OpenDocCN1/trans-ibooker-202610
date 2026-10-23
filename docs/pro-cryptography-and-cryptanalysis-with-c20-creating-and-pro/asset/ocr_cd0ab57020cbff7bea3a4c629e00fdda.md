```
unsigned long quadraticValueForY = PerformProjectionFor_X(
 z_yCoordinatePoint);
unsigned long quadraticValueForX = PerformProjectionFor_X(
 z_xCoordinatePoint);

// encoding process
unsigned int iV, oV, rV;

{  unsigned int result1, result2, result3, result4;
   iV=(bit_from_stream);

      // from the iteration of the 'y' quadratic
      result1=quadraticValueForY;

      // from the iteration of the 'x' quadratic
      result2=quadraticValueForX;

      // we will copy the bits if it is set in one operand but not
         both
      result3=iV^result1;
      result4=iV^result2;

      // the output value
      oV=result3;
      rV=result4; // branch in path to follow through IIM
}
bit_from_coding=(oV);

if ((rV) != 0)
{
      // use the route on the second root point
      z_xCoordinatePoint=-z_xCoordinatePoint;
      z_yCoordinatePoint=-z_yCoordinatePoint;
}
}
return bit_from_coding;
};
```