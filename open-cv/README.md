# OpenCV

## Read Image

```python
import cv2
img = cv2.imread(file_name)
```

## Convert to Grayscale

```python
import cv2
img = cv2.imread(file_name)
```

## Convert to HLS

```python
hls = cv2.cvtColor(img, cv2.COLOR_RGB2HLS)
H = hls[:,:,0] # hue
L = hls[:,:,1] # lightness
S = hls[:,:,2] # saturation
```

## Resize

```python
image = mpimg.imread('test_img.jpg')
small_img = cv2.resize(image, (32, 32))
```

