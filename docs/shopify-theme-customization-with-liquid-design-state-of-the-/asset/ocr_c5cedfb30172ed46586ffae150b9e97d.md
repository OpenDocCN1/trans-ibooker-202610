With model_viewer_tag in place, we have covered all four media types and ensured that we would represent each media type with an appropriate media tag. However, notice that we still have one final switch statement without any media tag.

We can consider the final switch statement as a failsafe if, for some reason, any of the previous switch statements or media fail to produce results, and we will use it with media_tag.

The media_tag filter is somewhat of a special kind of filter as this particular filter will automatically generate the appropriate media tag for any of the four previously mentioned media types. So, if we can use media_tag to generate all the media tags automatically, why don’t we use media_tag to generate all the media files? Why did we bother to learn about all the media tags until now?

While it is correct that media_tag will automatically detect each type of media type and render the appropriate media tag for it, by using media_tag, we will lose the ability to assign custom class names and attributes to specific media types. For this reason, we should never use media_tag as the primary option for rendering media assets. Instead, we should use it as a failsafe to correctly render the media assets on the storefront:

```
{ % assign product_object = all_products["gardening-gloves"] % }
{ % for media in product_object.media % }
  { % case media.media_type % }
    { % when "image" % }
      {{ media | img_tag: media.alt, "class1 class2", "300x300" }}
    { % when "external_video" % }
      { % if media.host == "youtube" % }
        {{ media | external_video_url: controls: 0, color: "white" | external_video_tag: class: "youtube_video" }}
      { % else % }
        {{ media | external_video_url: loop: 1, muted: 1 | external_video_tag: class: "vimeo_video" }}
      { % endif % }
    { % when "video" % }
      {{ media | video_tag: controls: true, image_size: "300x300", width: "300x" }}
    { % when "model" % }
      {{ media | model_viewer_tag }}
    { % else % }
```