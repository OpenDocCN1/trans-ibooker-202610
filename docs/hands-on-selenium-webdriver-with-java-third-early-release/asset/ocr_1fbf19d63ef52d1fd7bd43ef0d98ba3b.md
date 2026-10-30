for all browsers supported by Selenium WebDriver. Nevertheless, there are slight differences when using one or another browser. For instance, printing pages is only possible when using Chrome and Edge if the browser is started in headless mode. Regarding Firefox, this restriction is not required, and we can use Firefox as usual. Example 5-48 shows the test logic for printing a web page to PDF. You can find the complete tests for Firefox and headless Chrome/Edge in the examples repository.

# Example 5-48. Test printing a web page to PDF

```
@Test
void testPrint() throws IOException {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    PrintsPage pg = (PrintsPage) driver; ①
    PrintOptions printOptions = new PrintOptions();
    Pdf pdf = pg.print(printOptions); ②

    String pdfBase64 = pdf.getContent(); ③
    assertThat(pdfBase64).contains("JVBER"); ④

    byte[] decodedImg = Base64.getDecoder()
        .decode(pdfBase64.getBytes(StandardCharsets.UTF_8)); ⑤
    Path destinationFile = Paths.get("my-pdf.pdf");
    Files.write(destinationFile, decodedImg); ⑥
}
```

+   ① We cast the driver object to PrintsPage.
+   ② We print the current web page to PDF using the default configuration.
+   ③ We get the content of the PDF in Base64.
+   ④ We verify this content contains the file signature (the “magic word” JVBER).
+   ⑤ We convert the Base64 to a raw byte array.
+   ⑥ We write the PDF content (byte array) to a local file.

# The Input Method Engine (IME) API