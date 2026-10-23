```
string temporary;
theStringStream >> temporary;

return temporary; }

//** the function will show the content of the blocks in hex. We are
//** using this function in order to avoid changing the stream from
//** hexa to dec and reversed as well.
string displayAsBinary(unsigned long input320OrLessBitBlock) {
    bitset<8> theBitSet(input320OrLessBitBlock);
    return theBitSet.to_string(); }

//** based on the string, it will take the entire set of the
//** characters and converts them into ascii binary.
vector<unsigned long> binaryConversion(const string
inputOfAnyLength) {
    //** the vector used to store all the ascii characters
    vector<unsigned long> vectorBlockHoldingAsciiCharacters;

    //** take each character and convert the ascii character to
    //** the binary representation
    for (int i = 0; i < inputOfAnyLength.size(); ++i) {
        //** create a temporary variable. Use it to store the 8
        //** bit template for ascii value
        bitset<8> bitSetOf8Bits(inputOfAnyLength.c_str()[i]);

        //** the template of 8 bit add it into the block
        vectorBlockHoldingAsciiCharacters.
        push_back(bitSetOf8Bits.to_ulong());}

    return vectorBlockHoldingAsciiCharacters; }

//** get the ascii values stored as a vector in binary and add padding to
it in order to obtain a total of 512 bits.
vector<unsigned long> addPadOf512Bits(vector<unsigned long>
vectorBlockHoldingAsciiCharacters) {
    //** you can keep the variables names as given in the NIST
    //** for our implementation I have used my personal names for
    //** variables in order to get a uniqueness of the code
```