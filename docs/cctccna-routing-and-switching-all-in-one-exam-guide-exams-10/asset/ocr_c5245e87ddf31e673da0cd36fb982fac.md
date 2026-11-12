The next parameter wants the current time. This is based on UTC (unless you’ve changed the device’s time zone) and is in a 24-hour format. For example, 3 P.M. would be 15:00:00. Again, use context-sensitive help to figure out if more parameters are required:

```
IOS# clock set 15:00:00 ?
  <1-31>  Day of the month
  MONTH  Month of the year
IOS# clock set 15:00:00
```

Whenever you see a range of numbers inside angle brackets, you must choose a value in this range. If you see a parameter in all caps, such as MONTH, you must supply a name. In this clocking example, IOS wants the number of the day or name of the month, such as May—with this command, the router isn’t picky about in which order either of these parameters is entered, but in many cases the order does matter. Again, using context-sensitive help, indicate the number of the year following the name of the month:

```
IOS# clock set 15:00:00 23 May ?
  <1993-2035>  Year
IOS# clock set 15:00:00 23 May
```

Again, use context-sensitive help to see what’s next:

```
IOS# clock set 15:00:00 23 May 2004 ?
  <cr>
IOS# clock set 15:00:00 23 May 2004
IOS#
```

The <cr> means that you can press the ENTER key and the IOS device will accept the command. On an IOS router, use the show clock command to see your current time and date:

```
IOS# show clock
15:00:02.187 UTC Fri May 23 2003
IOS#
```

# Basic IOS Configuration