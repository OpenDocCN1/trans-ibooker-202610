
## Cryptanalysis Using Monogram, Bigram, and Trigram Frequency Counts

Frequency analysis is one of the best practices for finding the occurrence of characters in a ciphertext, with the goal of breaking the cipher. The analysis, based on pattern analysis, can be used to measure and count the characters as bigrams (or digraphs), a method for measuring pairs of characters that occur within the text. Trigram frequency analysis measures the occurrence of combinations formed out of three letters.

In this section, we will focus on text characterization with bigrams and trigrams that can be used for resolving ciphers, such as Playfair.

## Counting Monograms

Counting monograms is one of the most effective methods used in substitution ciphers, such as Caesar ciphers, Polybius squares, and so on. The method works very well because the English language has a specific frequency distribution. This also means that is not hidden by substitution ciphers. The distribution is shown in Figure 22-4.

![](img/c3d9424f64735276d661e7aba725b2a2_440_0.png)

Figure 22-4. Letter frequency for the English language

## Counting Bigrams

Bigrams counting is based on the same idea as counting monograms. Instead of counting the occurrence of single characters, you count the occurrence frequency for pairs of characters.

Figure 22-5 lists some of the common bigrams experienced during the cryptanalysis process. In Listing 22-2, we implemented a solution that counts the occurrences of bigrams. Figure 22-6 shows the output of this example of counting the bigrams. The source code from Listing 22-2 uses a file called bigram.txt which contains a sample text to illustrate the process of counting the bigrams.

Figure 22-5. Bigrams

```
TH 11699784
HE 10068926
IN 87674002
ER 77134382
AN 69775179
RE 60923600
ES 57070453
ON 56915252
ST 54018399
NT 50701084
EN 48991276
AT 48274564
ED 46647960
ND 46194306
TO 46115188
OR 45725191
EA 43329810
TI 42888666
AR 42353262
TE 42295813
NG 38567365
AL 38211584
IT 37938534
AS 37773878
IS 37349981
HA 35971841
ET 32872552
SE 31532272
OU 31112284
OF 30540904
```

Figure 22-6. Counting bigrams

## Listing 22-2. Counting Bigrams

```
#include <stdio.h>

int main(void)
{
    int alphabet_counting['z' - 'a' + 1]['z' - 'a' + 1] = {{ 0 }};
    int character0 = EOF, character1;
    FILE *fileBigramSampleText = fopen("bigram.txt", "r");

    if (fileBigramSampleText != NULL)
    {
        while (((character1 = getc(fileBigramSampleText)) != EOF))
        {
            if (character1 >= 'a' && character1 <= 'z' && character0 >= 'a'
            && character0 <= 'z')
            {
                alphabet_counting[character0 - 'a'][character1 - 'a']++;
            }
            character0 = character1;
        }
        fclose(fileBigramSampleText);
        for (character0 = 'a'; character0 <= 'z'; character0++)
        {
            for (character1 = 'a'; character1 <= 'z'; character1++)
```

```
char *position0 = NULL;
int character1;
FILE *fileTextForCountingBigrams = fopen("bigram.txt", "r");

memset(count, 0, sizeof(count));

if (fileTextForCountingBigrams != NULL)
{
    while ((character1 = getc(fileTextForCountingBigrams)) != EOF)
    {
        char *p1 = (char*)memchr(alphabet, character1, length_of_alphabet);
        if (p1 != NULL && position0 != NULL)
        {
            count[position0 - alphabet][p1 - alphabet]++;
        }
        position0 = p1;
    }
    fclose(fileTextForCountingBigrams);
    for (size_t i = 0; i < length_of_alphabet; i++)
    {
        for (size_t j = 0; j < length_of_alphabet; j++)
        {
            int n = count[i][j];
            if (n > 0)
            {
                printf("%c%c: %d\n", alphabet[i], alphabet[j], n);
            }
        }
    }
}
return 0;
}
```

## Counting Trigrams

Trigrams counting operates on the same principle as bigram counting; the difference consists in counting three characters.

## Conclusion

The chapter covered the concept of text characterization and showed its importance in the cryptanalysis process. You can now deal with the chi-squared statistic, and you can work with monograms, bigrams, and trigrams to decrypt substitution ciphertexts. As a summary, you learned about

- The concept of text characterization
- Working with monograms, bigrams, and trigrams
- Implementing the chi-squared statistic
- Monogram, bigram, and trigram implementations

## References

[1] Simon Singh, *The Code Book: The Science of Secrecy from Ancient Egypt to Quantum Cryptography*. ISBN 0-385-49532-3. 2000

[2] Helen F. Gaines, *Cryptanalysis: A Study of Ciphers and Their Solution*. 1989.

## CHAPTER 23

## Implementation and Practical Approach of Cryptanalysis Methods

The current chapter is a general discussion of the methodologies of cryptanalysis methods and how those methods can be applied in a quick and efficient way. The proposed methodologies are classic and actual (modern) cryptography/cryptanalysis algorithms and methods. Quantum cryptography is not included at this moment.

The methodology proposed (see Figure 23-1) is designed with the goal of helping you, the cryptanalyst, to know where you are situated during the cryptanalysis process. You can use the map presented in Figure 23-1 to choose the proper tool or method for your work.

Proceeding with the implementation of the cryptanalysis methods can be a very laborious task if you don’t have the proper information about the cryptographic method. The following will present a short process for identifying the necessary elements for conducting the cryptanalysis process. The cryptanalysis process consists of two general steps:

- *Step 1* is based on identifying what type of cryptanalysis should be conducted.
- *Step 2* consists of gathering everything that you know about cryptography algorithms. After these two steps have been performed properly and with maximum seriousness, you can move forward with the extra steps.
- *Step 3* is when you build a proper *attack model*.
- *Step 4* is when you choose the proper tools.

*Step 1.* This step deals with what kind of cryptanalysis should be performed. Within this step the cryptanalysis will decide within the business environment what role they will play: legal and authorized cryptanalyst, ethical hacker, or malicious cracker. As soon as you decide your role, you can move to Step 2.

*Step 2.* If the cryptanalyst is legitimate, they must be aware of two things before getting started: the *cryptography algorithm* and the *cryptographic key*. Based on the experience of some of cryptanalysts, this is not a necessary requirement but in some cases it will be very useful to know. As soon as you are aware of the cryptography algorithm and cryptographic key, you can easily start the cryptanalysis process by applying the proper methods and testing the security of the business applications.

*Step 3.* This step is based on setting up the attack model or attack type. Attack models and attack types will point out a quantitative variable used to indicate how much information a cryptanalyst will have access to when they perform the cracking methods on the encrypted message. The most important attacks are

- Ciphertext-only attack
- Known-plaintext attack
- Chosen-plaintext attack
- Chosen-ciphertext attack
  - Adaptive chosen-ciphertext attack
  - Indifferent chosen-ciphertext attack

Step 4. After the attack model has been picked or another model has been created and adapted properly with the case and requirements, you move to the next step, which is to pick the software tools. Choosing software tools from ones that already exist or creating your own tools can be time consuming but in practice will have massive contributions. Summarizing this, here are some tools that can be used in the cryptanalysis process, depending on what you’re trying to test.

- **Penetration tools:** Kali Linux, Parrot Security, BackBox
- **Forensics:** DEFT, CAINE, BlackArch, Matriux
- **Databases:** sqlmap (standalone version), Metasploit framework (standalone version), VulDB
