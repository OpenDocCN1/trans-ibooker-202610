Let’s show this with a polynomial from the last chapter. We don’t need to write any new code to make **pickle** work.

```
t = UniPoly(1, 't', 1)
p = (t**3 - 7)**5
p
```

t**15 - 35*t**12 + 490*t**9 - 3430*t**6 + 12005*t**3 - 16807

Now we *dump* and *load* the polynomials.

```
with open("work/pickle_example", "wb") as pickle_file:
    pickle.dump(p, pickle_file)

with open("work/pickle_example", "rb") as pickle_file:
    new_p = pickle.load(pickle_file)
```

They are equal:

```
p == new_p
```

True

The **dill** module extends **pickle** and handles class variables. [DILL] Like **pickle**, **dill** is Python-specific. The files they create may not work with Python versions significantly different from your own.

# 8.8.2 JSON

JSON is a standardized text-based interchange format created as part of the JavaScript programming language. “JSON” stands for “JavaScript Object Notation.” There is nothing special that ties it to JavaScript, and Python has good support for the format. [JSON]

JSON matches up well with Python data types, and Python has four primary functions for working with it.

The **src/examples/example.json** file contains an extended subset of my current Visual Studio Code settings file. JSON files should have the extension “.json”. Let’s read the example file with *load* and see what Python does with it.

```
import json

with open("src/examples/example.json", 'r') as input_file:
    json_ = json.load(input_file)
```