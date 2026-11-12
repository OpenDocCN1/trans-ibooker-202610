# 21. Create a new frame for the output stitched image with the following line of code:

`stitch = np.zeros((total_height, total_width, 4), np.uint8)`

# 22. Stitch the images into the frame created, in order of distance, and return the image mosaic obtained:

`for image in dist_sorted:
    offset_y = image[0][1] + cent_y
    offset_x = image[0][0] + cent_x
    end_y = offset_y + image[1].shape[0]
    end_x = offset_x + image[1].shape[1]
    stitch_cur = stitch[offset_y:end_y, offset_x:end_x, :4]
    stitch_cur[image[1]>0] = image[1][image[1]>0]

return stitch`

# 23. Now, implement a function that will create the output panorama image using the `create_mosaic()` function, given the input images to be stitched and the index of the central image (as center):

`def create_panorama(images, center):
    h,w,_ = images[0].shape
    f = 1000 # 800
    H = np.array([[f, 0, w/2], [0, f, h/2], [0, 0, 1]])
    for i in range(len(images)):
        images[i] = cylindrical_warp_image(images[i], H)`

# 24. First, stitch all of the images on the left side of the central image using the following code snippet:

`panorama = None
for i in range(center):
    print ('Stitching images {}, {}'.format(i+1, i+2))
    image_warped, image_origin = warp_image(images[i], \\
        homography(images[i + 1], images[i]))
    panorama = create_mosaic([image_warped, images[i+1]], \\
        [image_origin, (0,0)])
    images[i + 1] = panorama

#print ('Done left part')`

# 25. Stitch all of the images on the right side of the central image using the following code snippet. Return the panorama obtained:

`for i in range(center, len(images)-1):
    print ('Stitching images {}, {}'.format(i+1, i+2))
    image_warped, image_origin = warp_image(images[i+1], \\
        homography(images[i], images[i + 1]))
    panorama = create_mosaic([images[i], image_warped], \`