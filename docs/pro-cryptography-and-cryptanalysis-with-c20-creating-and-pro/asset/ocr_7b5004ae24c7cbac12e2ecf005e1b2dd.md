```
KeyGenerator keygen(seal_context);
PublicKey encryption_key = keygen.public_key();
SecretKey decryption_key = keygen.secret_key();
Encryptor bfv_encrypt(seal_context, encryption_key);
Evaluator bfv_evaluate(seal_context);
Decryptor bfv_decrypt(seal_context, decryption_key);

int value_x = 3;
Plaintext x_plaintext(to_string(value_x));
cout << "The value x = " + to_string(value_x) + " is expressed as a plaintext polynomial 0x" + x_plaintext.to_string() + "." << endl;

Ciphertext x_ciphertext;
cout << "Encrypting x_plaintext to x_ciphertext..." << endl;
bfv_encrypt.encrypt(x_plaintext, x_ciphertext);

cout << "  - the size of the x_ciphertext (freshly encrypted) is : " << x_ciphertext.size() << endl;
cout << "  - the noise budget for x_ciphertext is : " << bfv_decrypt.invariant_noise_budget(x_ciphertext) << " bits" << endl;

Plaintext value_x_decrypted;
cout << "  - decryption of x_encrypted: ";
bfv_decrypt.decrypt(x_ciphertext, value_x_decrypted);
cout << "0x" << value_x_decrypted.to_string() << endl;

cout << "Computing (x^2+2)..." << endl;
Ciphertext square_x_plus_two;
bfv_evaluate.square(x_ciphertext, square_x_plus_two);
Plaintext plain_value_two("2");
bfv_evaluate.add_plain_inplace(square_x_plus_two, plain_value_two);

cout << "  - the size of the square_x_plus_two is: " << square_x_plus_two.size() << endl;
cout << "  - the noise budget for square_x_plus_two is: " << bfv_decrypt.invariant_noise_budget(square_x_plus_two) << " bits" << endl;
```