35: #   54: 6   73: I   92: \   111: o
36: $   55: 7   74: J   93: ]   112: p
37: %   56: 8   75: K   94: ^   113: q
38: &   57: 9   76: L   95: _   114: r
39: '   58: :   77: M   96: `   115: s
40: (   59: ;   78: N   97: a   116: t
41: )   60: <   79: O   98: b   117: u
42: *   61: =   80: P   99: c   118: v
43: +   62: >   81: Q   100: d  119: w
44: ,   63: ?   82: R   101: e  120: x
45: -   64: @   83: S   102: f  121: y
46: .   65: A   84: T   103: g  122: z
47: /   66: B   85: U   104: h  123: {
48: 0   67: C   86: V   105: i  124: |
49: 1   68: D   87: W   106: j  125: }
50: 2   69: E   88: X   107: k  126: ~
51: 3   70: F   89: Y   108: l  127: DEL

To mimic BSD comm, I should print the lower value (B) first and draw another value from that file for the next iteration. The GNU version does the opposite. Note you should add use std::cmp::Ordering::* to your imports for this code:

```
let mut line1 = lines1.next();
let mut line2 = lines2.next();

loop {
    match (&line1, &line2) {
        (Some(val1), Some(val2)) => match
val1.cmp(val2) { ①
            Equal => { ②
                println!("{}", val1);
                line1 = lines1.next();
                line2 = lines2.next();
            }
            Less => { ③
                println!("{}", val1);
                line1 = lines1.next();
            }
            _ => { ④
                println!("{}", val2);
                line2 = lines2.next();
            }
        }
```