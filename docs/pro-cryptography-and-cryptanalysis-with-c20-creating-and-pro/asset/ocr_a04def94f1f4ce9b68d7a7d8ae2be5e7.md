```
```cpp
if (backpack0fBalls[0]==Blue&&backpack0fBalls[1]==Blue)
    ++bothAreBlue;
}

float probabilityOfFirstBallToBeBlue =
                    static_cast<float>(theFirstIsBlue) /
                                theTotal0fSamples;

float probabilityForBothBallsToBeBlue =
                         static_cast<float>(bothAreBlue) /
                                    theTotal0fSamples;

float probabilityForSecondBallToBeRed =
                         static_cast<float>(theSecondIsBlue) /
                                    theTotal0fSamples;

std::cout << "Probability for the first ball to be blue: "
          << probabilityOfFirstBallToBeBlue * 100.0 << "%\n" ;

std::cout<< "Probability for the second ball to be blue: "
          << probabilityForSecondBallToBeRed * 100.0 << "%\n" ;

std::cout << "Probability for both balls to be blue: "
          << probabilityForBothBallsToBeBlue * 100.0 << "%\n" ;
}
```
```

# Case Study 2: Computing the Probability Distribution

![](bbox=[0.117, 0.703, 0.849, 0.87])

*Figure 3-2.* Output of the probability distribution