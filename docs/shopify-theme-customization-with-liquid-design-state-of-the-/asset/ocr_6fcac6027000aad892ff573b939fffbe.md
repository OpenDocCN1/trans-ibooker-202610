Uploading a video as a product media asset is the same as uploading an image. To upload a video, click on the **Products** section within the admin sidebar and select the product for which we are uploading the video asset. Once inside, scroll down to the **Media** section and click on the **Add Media** button to start the uploading process.

> **Important note:**
> 
> Aside from external videos, uploading assets to the Shopify platform has specific limitations for all assets, including images and videos. Besides the specific types of formats that we can use, an image file cannot exceed the resolution of 20 MP, 20 MB in size, and must be in either `.jpeg` or `.png` format, whereas video files are limited to a maximum length of 60 seconds, 20 MB in size, and must be in either `.mp4` or `.mov` video format.

Once we have uploaded the video, we can also add optional information by including alt text and a poster image in the same way as for external media.

To output the video uploaded to product media files, we will need to use `video_tag`, accessible through the video object:

```
```liquid
{% assign product_object = all_products["gardening-gloves"] %}
{% for media in product_object.media %}
  {% case media.media_type %}
    {% when "image" %}
      {{ media | img_tag: media.alt, "class1 class2", "300x300" }}
    {% when "external_video" %}
      {% if media.host == "youtube" %}
        {{ media | external_video_url: controls: 0, color: "white" | external_video_tag }}
      {% else %}
        {{ media | external_video_url: loop: 1, muted: 1 | external_video_tag }}
      {% endif %}
    {% when "video" %}
      {{ media | video_tag }}
    {% when "model" %}
    {% else %}
  {% endcase %}
{% endfor %}
```
```