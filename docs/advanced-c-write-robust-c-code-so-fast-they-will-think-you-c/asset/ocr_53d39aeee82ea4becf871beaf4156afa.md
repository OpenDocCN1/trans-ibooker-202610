4. In the text editor, change the constructor of Voltage to be explicit:

```
struct Voltage
{
    explicit Voltage(float emf) : m_emf(emf)
    {
    }

    float m_emf;
};
```

5. Click on the Run button to recompile the code – now we get the following error:

![](bbox=[0.13, 0.33, 0.93, 0.57])

Figure 3.45: Failed conversion of int to Voltage

6. Remove the explicit from the constructor and change the calculate function to take a reference:

```
void calculate(Voltage& v)
```