6. Now, build the project and run the test. You should see this report:

![](bbox=[0.12, 0.11, 0.935, 0.33])

As you can see in the preceding figure, the first two tests passed and the last test failed. When we see this output, there are two options: either our project code is wrong, or the test is wrong. In this case, our test is wrong. This is because our CanSumAbsoluteValues test case expects that 6 is equal to sum(3, -3). This is because we assumed that our function sums up the absolute values of the integers provided. However, this is not the case. Our function simply adds the given numbers, whether they are positive or negative. Therefore, this test had a faulty assumption and failed.

7. Let's change the test and fix it. Change the test so that we expect the sum of -3 and 3 to be 0. Rename the test to reflect what this test actually does:

```
TEST_F(SumFuncTest, CanSumCorrectly) {
    EXPECT_EQ(7, sum(3, 4));
}
```

```
// change this part
TEST_F(SumFuncTest, CanUseNegativeValues) {
    EXPECT_EQ(0, sum(3, -3));
}
```