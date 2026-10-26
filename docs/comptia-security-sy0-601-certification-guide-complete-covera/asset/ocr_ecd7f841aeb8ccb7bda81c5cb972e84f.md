# **Example 2: _JavaScript – Setting the Day of the Month:_**

We will use JavaScript to set the day of the month. You will notice the JavaScript code between `<html>` tags: `<script>` to start the script and `</script>` to end the script. The var command is very common in JavaScript:

```
```html
<!DOCTYPE html>
<html>
<body>
<p>Click the button to display the date after changing the day of the month. </p>
<button onclick="myFunction()">Try it</button>
<p id="demo"></p>
<script>
function myFunction() {
var d = new Date(); d.setDate (15);
document.getElementById("demo").innerHTML = d;
}
</script>
</body>
</html>
```
```

An XSS attack can be identified by looking for the var command and for a variable with `<html>` tags `<script>` and `</script>`. Scripts with var are likely to be JavaScript. This is a very popular exam topic.

+   - **XML Injection:** XML is a language similar to HTML that is used with web data. The attacker will try and manipulate the XML script to carry out attacks. It is interoperable with Java and an XML injection attack is similar to XSS above. It can also be used to replace variables in the script.

**Example:** Equivalent XML Payload:

```
```xml
<?xmlversion="1.0"encoding="UTF-8"?><root><query>％3E％3Cscript％3Ealert(1,123)％3C/script％3E</query></root>
```
```

If you look at line two from `％3E％3Cscript％3E` through to `script％3E`, data is inserted into the XML script similar to XSS. You can prevent an XML injection attack by using input validation.