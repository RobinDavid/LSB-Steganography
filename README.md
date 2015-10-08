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

How to use it ?
---------------

Text steganography:

    from LSBSteg import *

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

    from LSBSteg import *

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

    from LSBSteg import *

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
