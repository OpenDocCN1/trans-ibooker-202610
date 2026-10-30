| Description | Maven | Gradle |
|---|---|---|
| Run tests starting with the word *Hello* | `mvn -B test -Dtest=Hello*` | `gradle test --tests Hello*` |
| Run tests that contain *Basic* or *Timeout* | `mvn test -Dtest=*Basic*,*Timeout*` | `gradle test --tests *Basic* --tests *Timeout*` |
| Run tests except those starting with *Firefox* | `mvn test -Dtest=!Firefox*` | `gradle test -PexcludeTests=**/Firefox*` |
| Run tests except those starting with *Docker* or containing *Remote* | `mvn test -Dtest=!Docker*,!*Remote*` | `gradle test -PexcludeTests=**/Docker*,**/*Remote*` |

In addition to the build tool, we can use built-in features provided by the unit testing frameworks for categorizing (also known as grouping or tagging) and filtering tests based on those categories. The following subsections explain how.

# JUnit 4