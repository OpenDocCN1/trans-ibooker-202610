```
cout<< endl << endl << "-------- Encrypting... --------";

cout<<endl<<"--Message--";
cout<<endl<<"m = "<<message;
cout<<endl<<"--Encryption f the message--";
cout<<endl<<"Enc(m) = (" << u << ", " << v << ")";

cout<< endl << endl << "-------- Decrypting... --------";
int result = (v - secretValue * u) % q;

int decryption;
if (result > q/2)
    decryption = 1;
else
    decryption  = 0;
cout<<endl<<"The message is: " << decryption;

return 0;
}
```

![](bbox=[0.1, 0.5, 0.93, 0.86])

Figure 13-2. The result of running the program of the public-key LWE example