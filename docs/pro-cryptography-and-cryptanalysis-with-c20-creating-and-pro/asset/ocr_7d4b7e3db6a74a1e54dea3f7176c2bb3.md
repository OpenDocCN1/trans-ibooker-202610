```
```cpp
cout << "MAJ(a,b,c): ox" << hex
     << MAJ(a,b,c) << "  dec:"
     << dec << MAJ(a,b,c) << "  sign:" << dec
     << (int)MAJ(a,b,c) << endl;
cout << "temporary_2 = 0x" << hex << temporary_2 << "  dec:" << dec <<
temporary_2 << "  sign:" << dec << (int)temporary_2 << endl; }

/** according to the NIST standard
    h = g;
    g = f;
    f = e;

/** Get the guarantee that we are still using 32 bits
    e = (d + temporary_1) & 0xFFFFFFFF;
    d = c;
    c = b;
    b = a;

/** Get the guarantee that we are still using 32 bits
    a = (temporary_1 + temporary_2) & 0xFFFFFFFF;

    /** display the content of each of the variable from
/** above according to the NIST standard.
    if (displayWorkVariablesForT) {
        cout << "t= " << t << " ";
        cout << displayAsHex (a) << " " << displayAsHex (b)
     << " " << displayAsHex (c) << " " << displayAsHex
(d) << " "   << displayAsHex (e) << " " << displayAsHex (f) << " " <<
displayAsHex (g) << " " << displayAsHex (h) << " " << endl; } }

/** display the content of each of the hash segment
    if(displayTheHashSegments) {
        cout << "InitialHashValueFor32Bit_0 = " << displayAsHex
(InitialHashValueFor32Bit_0) << " + " << displayAsHex (a) << " " <<
displayAsHex (InitialHashValueFor32Bit_0 + a) << endl;
```
```