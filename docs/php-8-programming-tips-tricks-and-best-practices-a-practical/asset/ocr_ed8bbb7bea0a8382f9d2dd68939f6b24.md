> **Tip**
> For more information on the GD library, see https://libgd.github.io/.

Let's now have a look at the impact of resource-to-object migration on the GD extension.

# GD extension resource-to-object migration

As with other PHP extensions that previously used *resources*, the GD extension has also primarily migrated from *resource* to *object*. As mentioned in the *PHP 8 extension resource-to-object migration* section, all of the `imagecreate*()` functions now produce GdImage objects rather than resources.

For an example of how this might present a code break after a PHP 8 migration, run these examples in two different browser tabs (on your local computer) and compare the difference. First, we run the PHP 7 example using this URL: http://172.16.0.77/ch07/php7_gd_is_resource.php. Here is the result:

![](bbox=[0.12, 0.43, 0.83, 0.68])

Figure 7.2 – PHP 7 GD image resource