```
a.removeAt(i);
a.removeAt(i);
i--;
} else if(a[i] == "-") {
a[i-1] = "${double.parse(a[i-1]) - double.parse(a[i+1])}";
a.removeAt(i);
a.removeAt(i);
i--;
}
}

if(a.length != 1) throw Error();

return double.parse(a[0]);
}
```

```
void deleteOne() {
  if(a.length > 0) {
    if(a.last.length > 1) {
      a.last = a.last.substring(0, a.last.length-1);
    } else {
      a.removeLast();
    }
  }
}

void deleteAll() => a = [];
}
```

# Make Your Own Changes

We have seen quite a lot of Dart code so far, but have you also been writing your own? If you haven’t, here is a chance for you to make a very simple and slight improvement to our calculator app.

The Calculation properly handles operator precedence, but it doesn’t support parentheses. That’s something you should be able to add on your own at this point by using additional regular expressions to find opening and closing parentheses and then evaluate expressions within the parentheses first.