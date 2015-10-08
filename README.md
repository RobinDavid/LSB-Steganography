LSB-Steganography
=================

Python program to steganography files into images using the Least Significant Bit technics.

I use the most basic method which is the LSB for Least Significant Bit. A colour pixel is composed of three component BGR all this three are encoded on one byte. So the idea is to store information in the first bit of every pixels component (BGR). By this way in the worse of case the decimal value is different of one which is not visible with human eyes on the image. In practice if you don't have the space to store all your datas in the first of every pixel you should start using the second and so on. You have to keep in mind that more your store data into an image more it can be detected.


Informations
--------------

LSBSteg module is based on OpenCV to hide datas in images. It uses the first bit (so the lowest) of every pixels and every colours
of an image. The code is quite simple to understand. If every first bits have been used the module start using the second so bigger the datas are more the image will be alterated.
To program all to hide everyhing, if there is enough space in the image. The main functions are:

* hideText: You provide directly your string and the program manage to hide it
* hideImage: You provide an OpenCV image and the method iterate every pixels to hide them. A good practice is to have a carrier 8 time bigger than the image to hide (so that every pixels will be put only in the first bit.
* hideBinary: You provide the path of the binary and it wil be hide. So this method can take any kind of files.


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


How to use it ?
---------------

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
