8. Now, we will add more test cases to make our code robust. This is generally good practice for writing code. First, create the test cases and make sure the code runs fine for new test cases and old test cases along with the correct functionality of the code. To add more test cases, add the following code to the **TestCases.cpp** file:

```
TEST(CheckIp, WrongTokenCount){
    testIp = "22.13111.11";
    ASSERT_FALSE(checkValidIp(testIp));
}
TEST(CheckIp, WrongTokenEmpty){
    testIp = "22.131..11";
    ASSERT_FALSE(checkValidIp(testIp));
}
TEST(CheckIp, WrongTokenStart){
    testIp = ".2.1.31.11";
    ASSERT_FALSE(checkValidIp(testIp));
}
TEST(CheckIp, WrongTokenEnd){
    testIp = "2.13.11.1.";
    ASSERT_FALSE(checkValidIp(testIp));
}
TEST(CheckIp, SpaceToken){
    testIp = "2.13.11. 1";
    ASSERT_FALSE(checkValidIp(testIp));
}
TEST(CheckIp, NonDigit){
    testIp = "2.13.b1.A1";
    ASSERT_FALSE(checkValidIp(testIp));
}
TEST(CheckIp, NonValidDigit){
    testIp = "2.13.521.61";
    ASSERT_FALSE(checkValidIp(testIp));
}
TEST(CheckIp, CorrectIp){
    testIp = "232.13.123.1";
    ASSERT_FALSE(checkValidIp(testIp));
}
```