p(x) = 3(x² + 2)(x + 1)², which means you will evaluate first (x² + 2), then (x + 1)² and then you will multiply the result between them and with 3. To compute (x² + 2), proceed as presented in Listing 12-10 (code added in seal_example_bfv function).

# Listing 12-10. Computing (x² + 2)

```
cout << "Computing (x^2+2)..." << endl;
Ciphertext square_x_plus_two;
bfv_evaluate.square(x_ciphertext, square_x_plus_two);
Plaintext plain_value_two("2");
bfv_evaluate.add_plain_inplace(square_x_plus_two,
    plain_value_two);

cout << "  - the size of the square_x_plus_two is: "
    << square_x_plus_two.size() << endl;
cout << "  - the noise budget for square_x_plus_two is: "
    << bfv_decrypt.invariant_noise_budget(square_x_plus_two)
    << " bits" << endl;

Plaintext decrypted_result;
cout << "  - decryption of square_x_plus_two: ";
bfv_decrypt.decrypt(square_x_plus_two, decrypted_result);
cout << "0x" << decrypted_result.to_string() << endl;
```

After running the application, you obtain the result from Listing 12-11 and Figure 12-7.

# Listing 12-11. The Result of Computing (x² + 2)

```
The value x = 3 is expressed as a plaintext polynomial 0x3.
Encrypting x_plaintext to x_ciphertext...
  - the size of the x_ciphertext (freshly encrypted) is : 2
  - the noise budget for x_ciphertext is : 55 bits
  - decryption of x_encrypted: 0x3
Computing (x^2+2)...
  - the size of the square_x_plus_two is: 3
  - the noise budget for square_x_plus_two is: 33 bits
  - decryption of square_x_plus_two: 0xB
```