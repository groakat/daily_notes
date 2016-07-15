# skimage: When saving a float image or converting it using `img_as_uint` warnings appear

Each time when an image with float datatype is saved, or converted to `uint`, a warning pops up:

    skimage.dtype_converter: WARNING: Possible precision loss when converting from float64 to uint8

This is particularly annoying if images are saved in a loop.

## Solution

Suppress warning like [0]

   import warnings
   import skimage
   
   with warnings.catch_warnings():
       warnings.simplefilter("ignore")
       skimage.io.imsave("test.png", all_white_image)


## References

[0] https://github.com/scikit-image/scikit-image/issues/543
 
