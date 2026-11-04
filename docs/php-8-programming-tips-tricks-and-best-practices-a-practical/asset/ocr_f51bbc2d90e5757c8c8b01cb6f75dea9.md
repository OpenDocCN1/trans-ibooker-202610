4. If a C compiler is not found, you will be warned. Also, you might need to install the PHP development library for your OS. The warning messages give you further guidance if this is the case.

5. Once completed, you then need to enable the extension. This is accomplished by adding `extension=swoole` to the `php.ini` file. If you are unsure of its location, use the `php -i` command and look for the location of the `php.ini` file. Here is a command you can issue from the command line that adds this directive:

```
echo "extension=swoole" >>/etc/php.ini
```

6. You can then use the following command to confirm the availability of the Swoole extension:

```
php --ri swoole
```

This concludes the installation of the Swoole extension. If you are custom-compiling PHP, you can also add the `--enable-swoole` option when running `configure` prior to compilation. This causes the Swoole extension to be compiled and enabled along with your core PHP installation (and allows you to bypass the installation steps just outlined). We will now have a look at a brief *Hello World* example taken from the documentation to test the installation.

# Testing the installation

The Swoole documentation provides a simple example you can use for a quick test to see if the installation was successful. The sample code is shown on the main Swoole documentation page (`https://www.swoole.co.uk/docs/`). We do not reproduce it here for copyright reasons. Here are the steps taken to run the *Hello World* test:

+   1. First, we copied the *Hello World* example from https://www.swoole.co.uk/docs/ to `/path/to/repo/ch12/php8_swoole_hello_world.php` file.

Next, we modified the demo program and changed `$server = new Swoole\HTTP\Server("127.0.0.1", 9501);` to `$server = new Swoole\HTTP\Server("0.0.0.0", 9501);`.

This change allows the Swoole server to listen on port 9501 for any **Internet Protocol (IP)** address.