# The response header

Upon receiving a request and processing its contents, the server may respond with a message such as the one shown here:

```
HTTP/1.1 200 OK
Cache-Control: private, max-age=0
Content-Length: 109264
Content-Type: text/html; charset=utf-8
Vary: Accept-Encoding
P3P: CP="NON UNI COM NAV STA LOC CURa DEVa PSAa PSDa OUR IND"
Set-Cookie: SRCHUID=A=F=NOFORM; domains=bing.com; expires=Tue, 10-Sep-2019 04:07:23 GMT; path=/
Set-Cookie: SRCHUIDV=2&GUID=30674151ABFF404A861581B0DE6F5FFC75d6mncHq1; domains=bing.com; expires=Tue, 10-Sep-2019 04:07:23 GMT; path=/
Set-Cookie: SRCHUID=D=20170910; domains=bing.com; expires=Tue, 10-Sep-2019 04:07:23 GMT; path=/
Set-Cookie: _SS=SID=27821837BF3962692F05I2CEBEP03SEC; domain=bing.com; path=/
Y_MSedge-Ref: Ref A: P0FFFC0AEF14589F9E6E5CEB003E3D0 Ref B: SYDEGE0412 Ref C: 2017-09-10T04:07:23Z
Set-Cookie: _EDGE_S=F=16SID=27821837BF3962692F05I2CEBEP03SEC; path=/; httponly; domain=bing.com
Set-Cookie: _EDGE_V=1; path=/; httponly; expires=Fri, 05-Oct-2018 04:07:23 GMT; domain=bing.com
Set-Cookie: MUIBD=27E4E5EF894C3C01439E567FA126225; path=/; expires=Fri, 05-Oct-2018 04:07:23 GMT; domain=bing.com
Set-Cookie: MUIBD=27E4E5EF894C3C01439E567FA126225; path=/; httponly; expires=Fri, 05-Oct-2018 04:07:23 GMT
Date: Sun, 10 Sep 2017 04:07:23 GMT
Connection: close

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"><html lang="es">
```

The first line of the response header contains the status code (200), which is a three-digit code. This helps the browser understand the status of operation. The following are the details of a few important fields:

+   - **Status code**: There is no field named status code, but the value is passed in the header. The 2xx series of status codes are used to communicate a successful operation back to the web browser. The 3xx series is used to indicate redirection when a server wants the client to connect to another URL when a web page is moved. The 4xx series is used to indicate an error in the client request and that the user will have to modify the request before resending. The 5xx series indicates an error on the server side, as the server was unable to complete the operation. In the preceding header, the status code is 200, which means that the operation was successful. A full list of HTTP status codes can be found at https://developer.mozilla.org/en-US/docs/Web/HTTP/Status.
+   - **Set-Cookie**: This field, if defined, will establish a cookie value in the client that can be used by the server to identify the client and store temporary data.
+   - **Cache-Control**: This indicates whether or not the contents of the response (images, script code, or HTML) should be stored in the browser's cache to reduce page loading times and how this should be done.