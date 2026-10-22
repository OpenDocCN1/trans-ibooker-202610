switch to using seconds instead of milliseconds. We do this to make the code more readable since either second or millisecond values are acceptable.

# Listing 3.10 Added animation iteration count

```
rect:nth-child(1) {
  fill: #1a9f8c;
  animation-delay: 0ms;
}

rect:nth-child(2) {
  fill: #1eab8d;
  animation-delay: 200ms;
}

rect:nth-child(3) {
  fill: #20b38e;
  animation-delay: 400ms;
}

rect:nth-child(4) {
  fill: #22b7 8d;
  animation-delay: 600ms;
}

rect:nth-child(5) {
  fill: #22b88e;
  animation-delay: 800ms;
}

rect:nth-child(6) {
  fill: #21b48d;
  animation-delay: 1s;
}

rect:nth-child(7) {
  fill: #1eaf8e;
  animation-delay: 1.2s;
}

rect:nth-child(8) {
  fill: #1ca48d;
  animation-delay: 1.4s;
}

rect:nth-child(9) {
  fill: #17968b;
  animation-delay: 1.6s;
}

rect:nth-child(10) {
  fill: #128688;
  animation-delay: 1.8s;
}
rect:nth-child(11) {
  fill: #128688;
  animation-delay: 2s;
}
```