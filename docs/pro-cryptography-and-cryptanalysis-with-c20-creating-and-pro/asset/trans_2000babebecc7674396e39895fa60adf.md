
//** the output of mask value
for(a = 1; a < 16; a++)
{
    //** the input of mask value
    for(b = 1; b < 16; b++)
    {
        //** input
        for(c = 0; c < 16; c++)
        {
            if (using_mask(c, b) ==
                using_mask(sBox_content[c], a))
            {
                approximation_array[b][a]++;
            }
        }
    }
}

void display_approximation()
{
    printf("Linear Approximations Values: \n");
    int a, b, c;
    for(a = 1; a < 16; a++)
        for(b = 1; b < 16; b++)
            if (approximation_array[a][b] == 14)
                printf(" %i : %i {--} %i\n",
                    approximation_array[a][b], a, b);
    printf("\n");
}

int round_function(int inputValue, int substitution_key)
{
    return sBox_content[inputValue ^ substitution_key];
}

void filling_known_numbers()
{
    int substitution_key_1 = rand() % 16;
    int substitution_key_2 = rand() % 16;

    printf("Generating data:  Key_1 = %i, Key_2 = %i\n", substitution_key_1, substitution_key_2);

    int total = 0;

    int c;
    for(c = 0; c < numbers_known; c++)
    {
        known_plaintext[c] = rand() % 16;
        known_ciphertext[c] = round_function(round_function(known_plaintext[c], substitution_key_1), substitution_key_2);
    }

    printf("Generating Data:  We have %i known pairs generated\n\n", numbers_known);
}

void keys_testing(int key_1, int key_2)      //testKeys
{
    int c;
    for(c = 0; c < numbers_known; c++)
    {
        if (round_function(round_function(known_plaintext[c], key_1), key_2) != known_ciphertext[c])
        {
            break;
        }
    }
    printf("# ");
}

int main()
{
    printf("Linear Cryptanalysis\n\n");

    srand(time(NULL));

    looking_for_approximation();
    display_approximation();

    int input_approximation = 11;
    int output_approximation = 11;

    /** how many numbers we known
    numbers_known = 16;
    filling_known_numbers();

    int cryptographic_key_score[16];
    int reaching_threshold = 0;

    printf("Proceeding with Linear Attack");
    printf("\tlinear Approximation = %i -> %i\n",
           input_approximation, output_approximation);

    printf("\n\n");

    int b, h;
    for(b = 0; b < 16; b++)
    {
        cryptographic_key_score[b] = 0;
        for(h = 0; h < numbers_known; h++)
        {
            reaching_threshold++;
            int middle_round = round_function(known_plaintext[h], b);

            if ((using_mask(middle_round, input_approximation)
                == using_mask(known_ciphertext[h],
                output_approximation)))
                cryptographic_key_score[b]++;

else
    cryptographic_key_score[b]--;
}
}

int maximum_score_value = 0;

for(b = 0; b < 16; b++)
{
    int score_value = cryptographic_key_score[b] *
                      cryptographic_key_score[b];
    if (score_value > maximum_score_value)
        maximum_score_value = score_value;
}

int good_cryptographic_keys[16];

for(h = 0; h < 16; h++)
    good_cryptographic_keys[h] = -1;

h = 0;
printf("Linear Attack:\n");
for(b = 0; b < 16; b++)
{
    if ((cryptographic_key_score[b] *
         cryptographic_key_score[b]) == maximum_score_value)
    {
        good_cryptographic_keys[h] = b;
        printf("\tPotential Value Candidate for
                 Cryptography Key 1 = %i\n",
               good_cryptographic_keys[h]);
        h++;
    }
}

int guessing_cryptographic_key_1;
int guessing_cryptographic_key_2;

for(h = 0; h < 16; h++)
{
    if (good_cryptographic_keys[h] != -1)
    {
        int cryptography_key_test_1 =
            round_function(known_plaintext[0],
            good_cryptographic_keys[h]) ^
            sBox_content_revision[
                known_ciphertext[0]];

        int tested = 0;
        int e;
        int bad = 0;
        for(e = 0;e < numbers_known; e++)
        {
            reaching_threshold += 2;
            int testOut = round_function(round_function
                (known_plaintext[e],
                good_cryptographic_keys[h]),
                cryptography_key_test_1);
            if (testOut != known_ciphertext[e])
                bad = 1;
        }
        if (bad == 0)
        {
            printf("\tFound Keys! K1 = %i, K2 = \n", good_cryptographic_keys[h],
                cryptography_key_test_1);
            guessing_cryptographic_key_1 =
                good_cryptographic_keys[h];
            guessing_cryptographic_key_2 =
                cryptography_key_test_1;
            printf("\tComputations Until Key Found = \n", reaching_threshold);
        }
    }
}
}
}

printf("\tThe total value of computations = %i\n\n", reaching_threshold);

reaching_threshold = 0;

printf("Brute Force Attack\n");
for(h = 0; h < 16; h++)
{
    for(b = 0; b < 16; b++)
    {
        int e;
        int bad = 0;
        for(e = 0;e < numbers_known; e++)
        {
            reaching_threshold += 2;
            int testOut = round_function(round_function(
                known_plaintext[e], h), b);
            if (testOut != known_ciphertext[e])
                bad = 1;
        }
        if (bad == 0)
        {
            printf("\tCryptographic Keys Found with
                Success! K1 = %i, K2 = %i\n", h, b);
            printf("\tThe number of computations
                computed until the cryptography
                key(s) were found = %i\n",
                reaching_threshold);
        }
    }
}
printf("\tComputations Total = %i\n", reaching_threshold);
}

# Conclusion

本章讨论了差分和线性密码分析攻击，以及这些攻击如何在实际中进行设计和实现。我们介绍了理论背景要素和主要基础，在设计此类密码分析攻击之前必须理解这些要素。

在本章结束时，你现在可以

- 在理论上识别密码分析师应重点关注的主要组件
- 理解这些组件的脆弱性如何以及如何被利用
- 实现线性和差分密码分析攻击

# References

[1] Joan Daemen, Lars Knudsen, and Vincent Rijmen “The Block Cipher Square. Fast Software Encryption (FSE)” in Volume 1267 of Lecture Notes in Computer Science (pp. 149–165). Haifa, Israel: Springer-Verlag. CiteSeerX 10.1.1.55.6109. 1997.

[2] H. Heys, “A Tutorial on Linear and Differential Cryptanalysis” in Cryptologia, vol. XXVI, no. 3 (pp. 189-221). 2002.

[3] M. Matsui, “Linear Cryptanalysis Method for DES Cipher” in Advances in Cryptology - EUROCRYPT ’93 (pp. 386-397). Springer-Verlag. 1994.

[4] E. Biham, “On Matsui’s linear cryptanalysis” in (ed. A. De Santis) Lecture Notes in Computer Science, vol. 950 (pp. 341–355). Springer-Verlag; Berlin. 1995.

[5] A. Biryukov, C. De Cannière, and M. Quisquater, “On Multiple Linear Approximations” in (ed. M. Franklin) Advances in Cryptology, proceedings of CRYPTO 2004, Lecture Notes in Computer Science 3152 (pp. 1-22). Springer-Verlag. 2004.

[6] L. Keliher, H. Meijer, and S.E. Tavares (2001). “New method for upper bounding the maximum average linear hull probability for SPNs” in (ed. B. Pfitzmann) LNCS, vol. 2045 (pp. 420–436). Springer-Verlag; Berlin. 2001.

[7] L.R. Knudsen and J.E. Mathiassen, “A chosen-plaintext linear attack on DES” in (ed. B. Schneier) Lecture Notes in Computer Science, vol. 1978 (pp. 262-272). Springer-Verlag; Berlin. 2001.

[8] M. Matsui and A. Yamagishi, “A new method for known plaintext attack of FEAL cipher” in (ed. R.A. Rueppel) Lecture Notes in Computer Science, vol. 658 (pp. 81-91). Springer-Verlag; Berlin. 1993.

[9] M. Matsui, “The first experimental cryptanalysis of the data encryption standard” in (ed. Y.G. Desmedt) Lecture Notes in Computer Science, vol. 839 (pp. 1-11). Springer-Verlag; Berlin. 1994.

[6] L. Keliher, H. Meijer, and S.E. Tavares (2001). “New method for upper bounding the maximum average linear hull probability for SPNs” in (ed. B. Pfitzmann) LNCS, vol. 2045 (pp. 420–436). Springer-Verlag; Berlin. 2001.

[7] L.R. Knudsen and J.E. Mathiassen, “A chosen-plaintext linear attack on DES” in (ed. B. Schneier) Lecture Notes in Computer Science, vol. 1978 (pp. 262-272). Springer-Verlag; Berlin. 2001.

[8] M. Matsui and A. Yamagishi, “A new method for known plaintext attack of FEAL cipher” in (ed. R.A. Rueppel) Lecture Notes in Computer Science, vol. 658 (pp. 81-91). Springer-Verlag; Berlin. 1993.

[9] M. Matsui, “The first experimental cryptanalysis of the data encryption standard” in (ed. Y.G. Desmedt) Lecture Notes in Computer Science, vol. 839 (pp. 1-11). Springer-Verlag; Berlin. 1994.

# CHAPTER 20

# Integral Cryptanalysis

积分密码分析是一种针对基于替代-置换网络构造的分组密码设计的密码分析技术。由于积分密码分析攻击可以针对 Square 分组密码 [1] 发起，因此也被称为 Square 攻击。它由 Lars Knudsen 设计。

分组密码的一个暴露点是替代-置换网络。当网络可以被发现时，密码分析对整个密码系统的负面影响就很高。分组密码的另一个暴露点是密钥本身以及密钥置换中涉及的表。当错误密钥与正确密钥相似或相同 时，系统就会被攻破。

在下一节中，我们将介绍可用于实现的关于分组密码的形式基础。进一步，我们将介绍启动积分密码分析攻击所需的元素，例如构建 Feistel 网络和为密码密钥生成置换表。一旦你对这两个阶段有清晰的理解，就很清楚积分密码分析必须如何进行。

# Basic Notions

要实现和设计积分密码分析攻击，在开始实现攻击之前非常重要的是要有形式要素。继续前进，让我们看一下以下概念作为主要起点。它们仅用于教育目的设计和实现此类攻击。

将 $(G, +)$ 视为一个有限阿贝尔群，其阶为 $k$。乘积群 $G^n = G \times ... \times G$ 是一个元素组，其结构为 $v = (v_1, ..., v_n)$，其中 $v_i \in G$。$G^n$ 内的加法定义为逐分量，因此，我们有 $u + v = w$ 对 $u, v, w \in G^n$ 成立，当且仅当对所有 $i$ 有 $u_i + v_i = w_i$。

