![](bbox=[0.18, 0.12, 0.88, 0.3])

Figure 10.20 Styled Radio Input

With our radio input styles, we will now turn our attention to shaping the checkmark inside of our checkbox.

# 10.3.8 Using CSS shapes to create the checkmark

Shaping our radio inputs was pretty simple, we used border-radius and achieved a disk shape. A checkmark is not quite as simple of a shape. To create it we will use clip-path.

> NOTE Clip-path Clip path allows us to create shapes by creating a clipping region that defines which part of the element should be shown versus hidden. We've used clip-path before in chapters 5 and 7 if you want a refresher

To create our checkmark, the shape we will apply to the clip-path is a polygon which will create our checkmark. Polygons are created by setting a series of X and Y percentage based coordinates between which a line is created. The (0, 0) coordinate is the top left corner of the shape. If the shape is not explicitly closed, it will automatically join the first and last points. Our polygon function will be: polygon(14% 44%, 0% 65%, 50% 100%, 100% 16%, 80% 0%, 43% 62%). Figure 10.21 explains the point by point construction of the shape.