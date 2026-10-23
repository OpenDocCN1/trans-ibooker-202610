```
//** the variable will store the length of the message in bits
int lengthOfMessageInBits = vectorBlockHoldingAsciiCharacters.size() * 8;

    int zeroesToAdd = 447 - lengthOfMessageInBits;

//** add another 8 bit block with the first bit being set to 1
    if(displayBlockStateAddOne)
        outputTheBlockState(vectorBlockHoldingAsciiCharacters);

    unsigned long t1Block = 0x80;
    vectorBlockHoldingAsciiCharacters.push_back(t1Block);

    if(displayBlockStateAddOne)
        outputTheBlockState(vectorBlockHoldingAsciiCharacters);
        outputTheBlockState(vectorBlockHoldingAsciiCharacters);

    //** we have 7 zeroes. We will need to substract 7 from
//** zeroesToAdd
    zeroesToAdd = zeroesToAdd - 7;

    //** debug mode. Find how much we need to get close to 512 bit
    if (displayDistanceFrom512Bit) {
        cout << "lengthOfMessageInBits = " <<
lengthOfMessageInBits << endl;
        cout << "zeroesToAdd = " << zeroesToAdd + 7 << endl; //
Plus 7 so this follows the paper. }

    //** debug mode
    if (displayDistanceFrom512Bit)
        cout << "adding " <<
zeroesToAdd / 8 << " empty eight bit blocks!\n";

//** add blocks of 8 bit length that will contains zero's
    for(int i = 0; i < zeroesToAdd / 8; i++)
        vectorBlockHoldingAsciiCharacters.push_back(0x00000000);

    //** we are finding ourself in 488 bits out 512 phase. Next
//** step is adding 1 in the binary representation in order to
//** form of eight bit blocks.
```