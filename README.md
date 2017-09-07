LSB-Steganography
=================

Python program based on stegonographical methods to hide files in images using the Least Significant Bit technique.

I used the most basic method which is the least significant bit. A colour pixel is composed of red, green and blue, encoded on one byte. The idea is to store information in the first bit of every pixel's RGB component. In the worst case, the decimal value is different by one which is not visible to the human eye. In practice, if you don't have space to store all of your data in the first bit of every pixel you should start using the second bit, and so on. You have to keep in mind that the more your store data in an image, the more it can be detected.


Information
-----------

LSBSteg module is based on OpenCV to hide data in images. It uses the first bit of every pixel, and every colour
of an image. The code is quite simple to understand; If every first bit has been used, the module starts using the second bit, so the larger the data, the more the image is altered.
The program can hide all of the data if there is enough space in the image. The main functions are:

* encode_text: You provide a string and the program hides it
* encode_image: You provide an OpenCV image and the method iterates for every pixel in order to hide them. A good practice is to have a carrier 8 times bigger than the image to hide (so that each pixel will be put only in the first bit).
* encode_binary: You provide a binary file to hide; This method can obfuscate any kind of file.

> *Only images without compression are supported*, namely not JPEG as LSB bits
might get tampered during the compression phase.

Installation
------------

This tool only require OpenCV and its dependencies.

```bash
pip install -r requirements.txt
```

Usage
-----

```bash
LSBSteg.py

Usage:
  LSBSteg.py encode -i <input> -o <output> -f <file>
  LSBSteg.py decode -i <input> -o <output>

Options:
  -h, --help                Show this help
  --version                 Show the version
  -f,--file=<file>          File to hide
  -i,--in=<input>           Input image (carrier)
  -o,--out=<output>         Output image (or extracted file)
```


Python module
-------------

Text encoding:

```python
#encoding
steg = LSBSteg(cv2.imread("my_image.png"))
img_encoded = steg.encode_text("my message")
cv2.imwrite("my_new_image.png", img_encoded)

#decoding
im = cv2.imread("my_new_image.png")
steg = LSBSteg(im)
print("Text value:",steg.decode_text())
```

Image steganography:

```python
#encoding
steg = LSBSteg(cv2.imread("carrier.png")
new_im = steg.encode_image(cv2.imread("secret_image.jpg"))
cv2.imwrite("new_image.png", new_im)

#decoding
steg = LSBSteg("new_image.png")
orig_im = steg.decode_image()
cv.SaveImage("recovered.png", orig_im)
```

Binary steganography:

```python
#encoding
steg = LSBSteg(cv2.imread("carrier.png"))
data = open("my_data.bin", "rb").read()
new_img = steg.encode_binary(data)
cv2.imwrite("new_image.png", new_img)

#decoding
steg = LSBSteg(cv2.imread("new_image.png"))
binary = steg.decode_binary()
with open("recovered.bin", "rb") as f:
    f.write(data)
    
```


License
-------

This software is MIT-Licensed.
