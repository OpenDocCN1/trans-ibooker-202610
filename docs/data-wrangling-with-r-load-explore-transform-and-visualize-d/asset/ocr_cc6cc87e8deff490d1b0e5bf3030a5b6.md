```
(awspandas) $cd aws-sdk-pandas
(awspandas) $ls
CODE_OF_CONDUCT.md          awswrangler               test_infra
CONTRIBUTING.md            building                  tests
CONTRIBUTING_COMMON_ERRORS.md  docs                  tox.ini
LICENSE.txt                fix.sh                    tutorials
NOTICE.txt                 poetry.lock               validate.sh
README.md                  pyproject.toml
THIRD_PARTY.txt            test.sh
(awspandas) $pip3 install .
Processing /Users/ssmsun/Documents/Initiatives/DataWranglerbook/code/aws-sdk-pandas
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting botocore<2.0.0,>=1.27.11
  Downloading botocore-1.27.82-py3-none-any.whl (9.2 MB)
     ---------------------------------------- 9.2/9.2 MB 25.5 MB/s eta 0:00:00
```

Figure 3.8: awswrangler installation on your local machine

4. Verify and start using the **awswrangler** custom installation:

+   1. Open a Python shell and import **awswrangler**.
+   2. Print the `__message__` and `__description__` variables to see whether we have installed the **awswrangler** package from our local machine/EC2 instance with the changes that were made in the earlier steps.

```
(awspandas) $python3
Python 3.9.12 (main, Apr  5 2022, 01:53:17)
[Clang 12.0.0 ] :: Anaconda, Inc. on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import awswrangler as wr
>>> print(wr.__description__)
Pandas on AWS.Updated for custom intallation.
>>> print(wr.__message__)
This was changed as a part of Learning exercise.
>>>
```

Figure 3.9: Verifying the awswrangler local installation with custom changes

5. As a final step, you can validate that the changes we made are only restricted to our virtual environment by moving out of the virtual environment and installing the **awswrangler** package using the `pip3 install awswrangler` command (which will install the package from the **pypi** remote repository without our custom code changes):