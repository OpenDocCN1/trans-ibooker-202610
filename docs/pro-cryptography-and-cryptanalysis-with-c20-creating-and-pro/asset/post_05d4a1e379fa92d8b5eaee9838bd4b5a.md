
- **Web and network:** Wireshark, Nmap, Nessus, Burp Suite, Nikto, and OpenVas
- **Other tools:** CrypTool (very useful and amazing tool)

The tools mentioned above represent a selection that are very used in practice and can produce desired results.

![](img/c3d9424f64735276d661e7aba725b2a2_450_0.png)

Figure 23-1. The cryptanalysis methodology

## Ciphertext-Only Attack

A ciphertext-only attack (COA) represents the weakest attack because it can easily be used by the cryptanalyst due to the fact that they just encoded the message.

The attacker-cryptanalyst has access to a set of ciphertexts. The attack is fully successful if the corresponding plaintexts are deduced together with the key.

In this type of attack (see Figure 23-2), the attacker/cryptanalyst will be able to observe the ciphertext. Everything that the cryptanalyst will see is represented by a set of scrambled and nonsense characters that create the output based on the encryption process.

![](img/c3d9424f64735276d661e7aba725b2a2_451_0.png)

Figure 23-2. COA representation

## Known-Plaintext Attack

The known-plaintext attack (KPA) helps the cryptanalyst to generate the ciphertext based on the fact that he is aware of the ciphertext.

The cryptanalyst follows a simple procedure by selecting the plaintext, but they will observe the pair that is compounded from the plaintext and ciphertext. The chance of success is better compared to COA. Simple ciphers are quite vulnerable to this attack. See Figure 23-3.

![](img/c3d9424f64735276d661e7aba725b2a2_452_0.png)

Figure 23-3. KPA representation

## Chosen-Plaintext Attack

In a chosen-plaintext attack (CPA), the cryptanalyst has the ability to select the plaintext that has been sent encrypted using an encryption algorithm and they can observe how the ciphertext is generated. This can be observed as an active model where the cryptanalyst has the chance to select the plaintext and to realize the encryption.

Based on the ability to select and pick any plaintext, the cryptanalyst has the chance to observe vital details about the ciphertext, which gives them a strong advantage in understanding how the algorithm works inside and the chance to get the secret key.

A professional cryptanalyst will have a strong database that contains known plaintexts, ciphertexts, and possible keys. In Listing 23-1 and Figure 23-5, we have provided an example of generating possible keys automatically. It is a very simple example for illustrating how possible keys can be generated. They can be used with the pairs to determine the cipher text input (see Figure 23-4).

![](img/c3d9424f64735276d661e7aba725b2a2_453_0.png)

Figure 23-4. CPA representation

### Listing 23-1. Automatic Generation of Random Keys

```
#include <stdio.h>
#include <time.h>
#include <iostream>

using namespace std;

//** generate an integer that is situated between 1 to 4
int generateInteger() {
    //** pseudo-random generator (srand).
    //** time(NULL) represents the seed
    srand(time(NULL));

    //** generate a random value and store
    //** the remainder of rand() to 5
    int randomValue = rand() % 5;

    //** if the value is equal with 0, move to the
    //** next value of i and return that value
    if (randomValue == 0)
        randomValue++;

return randomValue;
}

//** the function will generate randomly
//** an integer situated between 0 and 25
int generateRandomlyInteger(){
    //** pseudo-random generator (srand).
    //** time(NULL) represents the seed
    srand(time(NULL));

    //** generate a random value and store
    //** the remainder of rand() with 26
    int random_key = rand() % 26;
    return random_key;
}

//** based on the length provided, the function
//** will generate a cryptographic key
void generate_crypto_key(int length){
    //** create a string variable for cryptography
    //** key and initialize it with NULL
    string crypto_key = "";

    //** variable used for cryptography key generation
    string alphabet_lower_case = "abcdefghijklmnopqrstuvwxyz";
    string alphabet_upper_case = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    string special_symbols = "!@#$%&";
    string digits_and_numbers = "0123456789";

    //** local variables and their initializations
    int key_seed;
    int lowerCase_Alphabet_Count = 0;
    int upperCase_Alphabet_Count = 0;
    int digits_And_numbers_count = 0;
    int special_symbols_count = 0;

    //** the variable count will save the length
    //** of the cryptography key.
    //** initially we will set it to zero
int countingLengthCryptoKey = 0;
while (countingLengthCryptoKey < length) {
    /** generateInteger() function will return a number that
    /** is situated between 1 and 4.
    /** The number that is generated will be used in
    /** assignation with one of the strings that has been
    /** defined above (for example: alphabet_lower_case,
    /** alphabet_upper_case, special_symbols, and
    /** digits_and_numbers).
    /** This being said, the following correspondence will
    /** be applied: (1) for alphabet_lower_case, (2) for
    /** alphabet_upper_case, (3) for special_symbols, and
    /** (4) digits_and_numbers
    int string_type = generateInteger();

    /** For the first character of the cryptography key we
    /** will put a rule in such way that it should be a
    /** letter, in such way that the string that will be
    /** selected will be an lower case alphabet or an upper
    /** case alphabet. The IF condition is quite vital as
    /** the switch is based on it and the value that
    /** string_type variable will have.
    if (countingLengthCryptoKey == 0) {
        string_type = string_type % 3;
        if (string_type == 0)
            string_type++;
    }

    switch (string_type) {
        case 1:
            /** based on the IF condition, it is
            /** necessary to check the minimum
            /** requirements of the lower case alphabet
            /** characters if they have been accomplished
            /** and fulfilled. If we are dealing with the
            /** situation in which the requirement has
            /** not been achieved we will situate ourself
            /** in the break phase.
if ((lowerCase_Alphabet_Count == 2)
    && (digits_And_numbers_count == 0
    || upperCase_Alphabet_Count == 0
    || upperCase_Alphabet_Count == 1
    || special_symbols_count == 0))
    break;

key_seed = generateRandomlyInteger();
crypto_key = crypto_key +
    alphabet_lower_case[key_seed];
lowerCase_Alphabet_Count++;
countingLengthCryptoKey++;
break;

case 2:
    //** based on the IF condition, it is
    //** necessary to check the minimum
    //** requirements of the upper case alphabet
    //** characters if they have been accomplished
    //** and fulfilled. If we are dealing with the
    //** situation in which the requirement has
    //** not been achieved we will situate ourself
    //** in the break phase.
    if ((upperCase_Alphabet_Count == 2)
        && (digits_And_numbers_count == 0
        || lowerCase_Alphabet_Count == 0
        || lowerCase_Alphabet_Count == 1
        || special_symbols_count == 0))
        break;
    key_seed = generateRandomlyInteger();
    crypto_key = crypto_key +
        alphabet_upper_case[key_seed];
    upperCase_Alphabet_Count++;
    countingLengthCryptoKey++;
    break;

case 3:
    /** based on the IF condition, it is
     ** necessary to check the minimum
     ** requirements of the numbers if they have
     ** been accomplished and fulfilled. If we
     ** are dealing with the situation in which
     ** the requirement has not been achieved we
     ** will situate ourself in the break phase.
     */
    if ((digits_And_numbers_count == 1)
            && (lowerCase_Alphabet_Count == 0
            || lowerCase_Alphabet_Count == 1
            || upperCase_Alphabet_Count == 1
            || upperCase_Alphabet_Count == 0
            || special_symbols_count == 0))
        break;
    key_seed = generateRandomlyInteger();
    key_seed = key_seed % 10;
    crypto_key = crypto_key +
                 digits_and_numbers[key_seed];
    digits_And_numbers_count++;
    countingLengthCryptoKey++;
    break;

case 4:
    /** based on the IF condition, it is
     ** necessary to check the minimum
     ** requirements of the special characters if
     ** they have been accomplished and
     ** fulfilled. If we are dealing with the
     ** situation in which the requirement has
     ** not been achieved we will situate ourself
     ** in the break phase.
     */
```
