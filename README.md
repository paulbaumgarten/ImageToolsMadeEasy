# ImageToolsMadeEasy

Tools to simplify using face detection and ArUco recognition with PIL image objects

## About

This brings a range of CV2 image functionlaity into a form that is easy to use with PIL Image objects. The module was created to allow me to introduce image based functionality to my students while keeping things as simple as possible (switching between PIL and CV2 was an unnecessary complication I felt).

Key functions currently provided are:

* Taking a photo with the built in camera, returning a PIL image
* Face detection within a PIL image
* ArUco marker detection within a PIL image

## Usage: Camera

A basic example

```python
camera = ImageTools.Camera()
img = camera.take_photo()
img.save("my photo.png", "png")
img.show()
```

## Usage: Face detection

For the face detection function to work, you must supply a file path to a haarcascade file. You can obtain the relevant file from [https://github.com/opencv/opencv/tree/master/data/haarcascades](https://github.com/opencv/opencv/tree/master/data/haarcascades)

The `ImageTools.get_faces()` function will return a list of lists. The outer list represents the number of faces seen, where as the inner list represent the coordinates for an individual face being the `[left, top, width, height]` pixel values.

To get the coordinates of each face in an image

```python
from PIL import Image
import ImageTools
# ...
camera = ImageTools.Camera()
img = camera.take_photo()
faces = ImageTools.get_faces(img, "haarcascade_frontalface_default.xml")
print(faces)
```

To obtain seperate jpg image for each face detected

```python
from PIL import Image
import ImageTools
# ...
counter = 0
camera = ImageTools.Camera()
img = camera.take_photo()
faces = ImageTools.get_faces(img, "haarcascade_frontalface_default.xml")
for a_face in faces: # for each individual face in the list of faces...
    x,y,w,h = a_face # extract the left, top, width and height locations of a face
    a_face_img = img.crop((x,y,x+w,y+h))
    a_face_img.save(f"face_{counter:2}.jpg", "jpg")
    a_face_img.show()
    counter = counter + 1
```

To use a drawing tool to put rectangles highlighting the faces found in the original image...

```python
from PIL import Image, ImageDraw
import ImageTools
# ...
camera = ImageTools.Camera()
img = camera.take_photo()
draw = ImageDraw.Draw(img)          # create the drawing object
faces = ImageTools.get_faces(img, "haarcascade_frontalface_default.xml")
for a_face in faces: # for each individual face in the list of faces...
    x,y,w,h = a_face # extract the left, top, width and height locations of a face
    draw.rectangle((x,y,x+w,y+h), outline="#ffff00", width=5)   # draw a rectangle around the face
# show the final image, highlighting each face
img.show()
```

## Uuage: ArUco markers

The `ImageTools.get_aruco()` function when provided a PIL Image object as a parameter, will return a Python list of the ArUco markers it detected within the image.

```python
from PIL import Image
import ImageTools
# ....
camera = ImageTools.Camera()
image = camera.take_photo()
markers = ImageTools.get_aruco(image)
if 70 in markers:
    print("I saw ArUco marker 70")
if 71 in markers:
    print("I saw ArUco marker 71")
if 72 in markers:
    print("I saw ArUco marker 72")
```

* ArUco Markers are simple black and white printed codes (think of them as QR-code-lite) that resolve to an integer number.
* The OpenCV library has [built in functionality](https://docs.opencv.org/trunk/d5/dae/tutorial_aruco_detection.html) for recognising these markers.  
* The Python implementation of OpenCV detection was based on [this stackoverflow](https://stackoverflow.com/questions/52814747/aruco-opencv-example-all-markers-rejected)
* Aruco markers I printed to test where were generated from [here](https://docs.opencv.org/trunk/d5/dae/tutorial_aruco_detection.html)
* The code has been designed for the "4x4_1000" style of aruco markers but you can easily change that in the `camera_got_image` function if you wish


## Install

```bash
pip install ImageToolsMadeEAsy
```

## Dependencies

These should all be installed for you automatically, so are just provided for informational purposes.

* `opencv-contrib-python`
* `numpy`
* `PIL`

## Author

Paul Baumgarten 2019 @ [https://github.com/paulbaumgarten/ImageToolsMadeEasy](https://github.com/paulbaumgarten/ImageToolsMadeEasy)

