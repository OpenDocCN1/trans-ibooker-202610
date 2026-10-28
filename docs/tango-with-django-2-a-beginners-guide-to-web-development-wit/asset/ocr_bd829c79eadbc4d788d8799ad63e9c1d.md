# ## 4.3 Serving Media

Static media files can be considered files that don’t change and are essential to your application. However, often you will have to store *media files* which are dynamic. These files can be uploaded by your users or administrators, and so they may change. As an example, a media file would be a user’s profile picture. If you run an e-commerce website, a series of media files would be used as images for the different products that your online shop has.

To serve media files successfully, we need to update the Django project’s settings. This section details what you need to add – [but we won’t be fully testing it out until later](#) where we implement the functionality for users to upload profile pictures.

# ### Serving Media Files

![](bbox=[0.126, 0.411, 0.174, 0.445])

Like serving static content, Django provides the ability to serve media files in your development environment. This allows you to test and make sure everything is working. The methods that Django uses to serve this content are highly unsuitable for a production environment, so you should be looking to host your app’s media files by some other means (through a production-grade web server like Apache). The [deployment chapter](#) will discuss this in more detail.

# ## Modifying `settings.py`

First, open your Django project’s `settings.py` module. In here, we’ll be adding a couple more things. Like static files, media files are uploaded to a specified directory on your filesystem. We need to tell Django where to store these files.

At the top of your `settings.py` module, locate your existing `BASE_DIR`, `TEMPLATE_DIR` and `STATIC_DIR` variables — they should be close to the top. Underneath, add a further variable, `MEDIA_DIR`.