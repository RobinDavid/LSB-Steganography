LSB-Steganography
=================

Python program based on stegonographical methods to hide files in images using the Least Significant Bit technique.

I used the most basic method which is the least significant bit. A colour pixel is composed of red, green and blue, encoded on one byte. The idea is to store information in the first bit of every pixel's RGB component. In the worst case, the decimal value is different by one which is not visible to the human eye. In practice, if you don't have space to store all of your data in the first bit of every pixel you should start using the second bit, and so on. You have to keep in mind that the more your store data in an image, the more it can be detected.


Information
-----------

LSBSteg module is based on OpenCV to hide data in images. It uses the first bit of every pixel, and every colour
of an image. The code is quite simple to understand; If every first bit has been used, the module starts using the second bit, so the larger the data, the more the image is altered.
The program can hide all of the data if there is enough space in the image. The main functions are:

* hideText: You provide a string and the program hides it
* hideImage: You provide an OpenCV image and the method iterates for every pixel in order to hide them. A good practice is to have a carrier 8 times bigger than the image to hide (so that each pixel will be put only in the first bit).
* hideBinary: You provide a binary file to hide; This method can obfuscate any kind of file.

Installation
------------

This tool only require OpenCV and its dependencies.

```bash
pip install -r requirements.txt
```



Command-line
------------

```bash
> python LSBSteg.py -h
usage: LSBSteg.py [-h] [-image IMAGE] [-binary BINARY] [-steg-out STEG_OUT]
                  [-steg-image STEG_IMAGE] [-out OUT]

This python program applies LSB Steganography to an image and some type of
input

optional arguments:
  -h, --help            show this help message and exit

Hide binary with steg:
  -image IMAGE          Provide the original image
  -binary BINARY        The binary file to be obfuscated in the image
  -steg-out STEG_OUT    The resulting steganographic image

Reveal binary:
  -steg-image STEG_IMAGE
                        The steganographic image
  -out OUT              The original binary
```

Hide the message:
```bash
> python LSBSteg.py -image original.png -binary original.bin -steg-out steg.png
```

Reveal the hidden message:
```bash
> python LSBSteg.py -steg-image steg.png -out bin
```

Confirm:
```bash
> cat original.bin | md5
42ABC...
> cat bin | md5
...
```


Python module
-------------

Text steganography:

    from LSBSteg import LSBSteg

    #Hide
    str = "Hello World"
    carrier = cv.LoadImage("image.jpg")
    steg = LSBSteg(carrier)
    steg.hideText(str)
    steg.saveImage("res.png") #Image that contain datas

    #Unhide    
    im = cv.LoadImage("res.png")
    steg = LSBSteg(im)
    print "Text value:",steg.unhideText()

Image steganography:

    from LSBSteg import LSBSteg

    #Hide
    imagetohide = cv.LoadImage("secret_image.jpg")
    carrier = cv.LoadImage("image.jpg")
    steg = LSBSteg(carrier)
    steg.hideImage(imagetohide)
    steg.saveImage("res.png")

    #Unhide
    inp = cv.LoadImage("res.png")
    steg = LSBSteg(inp)
    dec = steg.unhideImage()
    cv.SaveImage("recovered.png", dec) #Image retrieve into the other image

Binary steganography:

    from LSBSteg import LSBSteg

    #Hide
    carrier = cv.LoadImage("image.jpg")
    steg = LSBSteg(carrier)
    steg.hideBin("ls") #I took the first binary I found
    steg.saveImage("res.png")

    #Unhide
    inp = cv.LoadImage("res.png")
    steg = LSBSteg(inp)
    bin = steg.unhideBin()
    f = open("res","wb") #Write the binary back to a file
    f.write(bin)
    f.close()


License
-------

This software is MIT-Licensed.
