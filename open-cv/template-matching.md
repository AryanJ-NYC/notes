# Template Matching

{% embed url="http://opencv-python-tutroals.readthedocs.io/en/latest/py\_tutorials/py\_imgproc/py\_template\_matching/py\_template\_matching.html" %}

```python
import cv2
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
import numpy as np

# image = mpimg.imread('bbox-example-image.jpg')
image = mpimg.imread('temp-matching-example-2.jpg')
templist = ['cutout1.jpg', 'cutout2.jpg', 'cutout3.jpg',
            'cutout4.jpg', 'cutout5.jpg', 'cutout6.jpg']

# Here is your draw_boxes function from the previous exercise
def draw_boxes(img, bboxes, color=(0, 0, 255), thick=6):
    # Make a copy of the image
    imcopy = np.copy(img)
    # Iterate through the bounding boxes
    for bbox in bboxes:
        # Draw a rectangle given bbox coordinates
        cv2.rectangle(imcopy, bbox[0], bbox[1], color, thick)
    # Return the image copy with boxes drawn
    return imcopy
    
    
# Define a function that takes an image and a list of templates as inputs
# then searches the image and returns the a list of bounding boxes 
# for matched templates
def find_matches(img, template_list):
    # Make a copy of the image to draw on
    # Define an empty list to take bbox coords
    bbox_list = []
    # Iterate through template list
    for template_path in template_list:
        # Read in templates one by one
        template = mpimg.imread(template_path)
        width, height = template.shape[:-1]
        # Use cv2.matchTemplate() to search the image
        res = cv2.matchTemplate(img, template, cv2.TM_CCOEFF_NORMED)
        #     using whichever of the OpenCV search methods you prefer
        # Use cv2.minMaxLoc() to extract the location of the best match
        min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(res)
        # Determine bounding box corners for the match
        top_left = max_loc
        bottom_right = (top_left[0]+height, top_left[1]+width)
        bounding_box = (top_left, bottom_right)
        # Return the list of bounding boxes
        bbox_list.append(bounding_box)
    return bbox_list

bboxes = find_matches(image, templist)
result = draw_boxes(image, bboxes)
plt.imshow(result)
```

