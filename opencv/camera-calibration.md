# Camera Calibration

## Radial + Tangential Distortion

```python
import cv2
import glob
import numpy as np
import matplotlib.image as mpimg

nx, ny = (8, 6)

images =glob.glob('../path/to/files/calibration*.jpg')

# Arrays to store object points and image points from all the images
objpoints = [] # 3D points in real world space
imgpoints = [] # 2D points in image plane

# Prepare object points, like (0, 0, 0), ... (7, 5, 0)
objp = np.zeros((nx*ny, 3), np.float32)
objp[:, :2] = np.mgrid[0:nx, 0:ny].T.reshape(-1, 2) #x, y coordinates

for fname in images:
  img = mpimg.imread(fname)
  gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
  
  # Find the chessboard corners
  ret, corners = cv2.findChessboardCorners(gray, (nx, ny), None)
  
  # If corners found, add object points, image points
  if ret:
    imgpoints.append(corners)
    objpoints.append(objp)
  
    # Draw and display the corners
    img = cv2.drawChessboardCorners(img, (nx, ny), corners, ret)

# Calibration
# mtx - camera matrix needed to transform 3D obj points to 2d image points
# dist -  distortion coefficients
# rvecs, tvecs - rotation and translation vectors
ret, mtx, dist, rvecs, tvecs = cv2.calibrateCamera(objpoints, imgpoints, gray.shape[::-1], None, None)

# img - distorted image
# dst - destination image
dst = cv2.undistort(img, mtx, dist, None, mtx)
```

## Perspective Transform

```python
def warp(img):
  # Define calibration box in source (original) and destination (desired or warped) coordinates
  img_size = (img.shape[1], img.shape[0])
  
  # four source coordinates
  src = np.float32(
    [[850, 320],
     [865, 450],
     [533, 350],
     [535, 210]])
   
  # four desired coordinates
  dst = np.float32(
     [[870, 240],
      [870, 370],
      [520, 370],
      [520, 240]])
    
  # compute the perspective transfrom, M
  M = cv2.getPerspectiveTransform(src, dst)

  # compute the inverse by swapping the input parameters
  Minv = cv2.getPerspectiveTransform(dst, src)

  # create warped images - uses linear interpolation
  warped = cv2.warpPerspective(img, M, img_size, flags=cv2.INTER_LINEAR)
  
  return warped
```

