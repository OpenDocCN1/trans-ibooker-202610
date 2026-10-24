```
```javascript
calc = Calculation();
});
test('simple addition', () {
  calc.add("5");
  calc.add("+");
  calc.add("6");
  calc.add("5");
  expect(calc.getResult(), 70.0);
});
test('more sums', () {
  calc.add("5");
  calc.add("5");
  calc.add("-");
  calc.add("5");
  calc.add("+");
  calc.add("50");
  expect(calc.getResult(), 100.0);
});
test('simple multiplication', () {
  calc.add("5");
  calc.add("x");
  calc.add("6");
  expect(calc.getResult(), 30.0);
});
test('division', () {
  calc.add("5");
  calc.add("÷");
  calc.add("2");
  expect(calc.getResult(), 2.5);
});
test('precedence', () {
  calc.add("5");
  calc.add("+");
  calc.add("6");
  calc.add("x");
  calc.add("5");
  expect(calc.getResult(), 35.0);
});
test('string', () {
  calc.add("5");
  calc.add("x");
  calc.add("6");
  calc.add("+");
  calc.add("7");
  expect(calc.getString().toString(), "5x6+7");
});
}
```
```

# Make Your Own Changes

If you have implemented parentheses in the calculator app, you should consider writing a unit test for that too.

It is very easy to write unit tests: you actually only need to split calc.add calls when you want to add an operator, but it’s recommended you add numbers one digit at a time at least in some tests since that’s how the actual calculator app does it and there might be an issue that only manifests itself when doing it that way (or maybe adding more than one digit at a time would break an implementation that would work in the real calculator, but that’s not the case for how we implemented it).